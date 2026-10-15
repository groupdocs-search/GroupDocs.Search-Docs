---
id: mcp-uc-search-documents-with-ai-agents
url: search/mcp/use-cases/search-documents-with-ai-agents
title: How to search documents with AI agents using MCP
linkTitle: Search with AI agents
weight: 1
description: "Search a folder of PDF, Word, Excel and email files with an AI agent over MCP, locally: index once, find the matching documents, and quote the passages."
keywords: search documents AI agent, MCP full-text search, Claude search local files, index folder for AI, search PDF Word email locally
productName: GroupDocs.Search MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to search documents with AI agents using MCP"
        description: "Search a folder of PDF, Word, Excel and email files with an AI agent over MCP, locally: index once, find the matching documents, and quote the passages."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Search MCP server with Docker and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Index the contracts folder, then tell me which contracts mention a termination penalty and quote the clause."
---

Run the GroupDocs.Search MCP server locally in Docker, ask your agent to index a folder, and it can answer *which documents say X* and *what exactly they say* — three tool calls, no upload:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

> Index the contracts folder, then tell me which contracts mention a termination penalty and quote the clause.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* Docker, and the server registered in your AI client — [quick start]({{< ref "search/mcp/getting-started/_index.md" >}}).
* The folder mounted at `/data`. On Linux, add `--user "$(id -u):$(id -g)"`.

## The pattern

```text
user              agent                         Search MCP server
 |  "Which contracts mention a termination penalty?"           |
 |---------------->|                                           |
 |                 |-- add_to_index ------------------------->|  reads the
 |                 |   {"documentsPath":"contracts",           |  files once,
 |                 |    "indexName":"contracts"}               |  stores text
 |                 |<- "5 document(s) added, 5 in the index" --|
 |                 |-- search ------------------------------->|
 |                 |   {"query":"termination AND penalty"}     |
 |                 |<- {"documentsFound":3, "documents":[ ... |  which files
 |                 |     "contracts/acme-...-agreement.pdf",   |
 |                 |     "contracts/globex-...-agreement.docx",|
 |                 |     "contracts/initech-nda.docx" ]}       |
 |                 |-- get_text_fragments ------------------->|
 |                 |   {"query":"\"termination penalty\""}     |
 |                 |<- === contracts/acme-...-agreement.pdf ===|  the passages
 |                 |   ...a termination penalty of three       |
 |                 |   monthly fees applies...                 |
 |<- "Two contracts carry one: Acme (3 monthly fees, 12.3),    |
 |    Globex (50% of the remaining fees). Initech excludes it."|
```

1. **Index once** — [`add_to_index`]({{< ref "search/mcp/tools-reference/add-to-index.md" >}}) reads the files and stores their text in a named index.
2. **Find the files** — [`search`]({{< ref "search/mcp/tools-reference/search.md" >}}) returns the matching documents as JSON.
3. **Quote the passages** — [`get_text_fragments`]({{< ref "search/mcp/tools-reference/get-text-fragments.md" >}}) returns each match with words of context.

## What the agent sees

Captured from server 26.10.1 against five synthetic contracts (evaluation note trimmed):

```text
-> add_to_index {"documentsPath":"contracts","indexName":"contracts"}
<- Indexed 'contracts' into index 'contracts': 5 document(s) added, 5 in
   the index.

-> search {"query":"termination AND penalty","indexName":"contracts"}
<- { "documentsFound": 3, "totalOccurrences": 9, "documents": [
     { "filePath": "contracts/acme-master-services-agreement.pdf", ... },
     { "filePath": "contracts/globex-consulting-agreement.docx", ... },
     { "filePath": "contracts/initech-nda.docx", ... } ] }

-> get_text_fragments {"query":"\"termination penalty\"",
                       "indexName":"contracts"}
<- === contracts/acme-master-services-agreement.pdf (1 occurrence(s)) ===
     [content] …terminates before the end of the Term, a termination
   penalty of three monthly fees applies, payable within 30…
   === contracts/globex-consulting-agreement.docx (1 occurrence(s)) ===
     [content] …Early termination penalty: the Client pays 50 percent of
   the fees…
```

The agent's answer: *two contracts carry a termination penalty — Acme (three monthly fees, clause 12.3) and Globex (50% of the remaining fees); the Initech NDA mentions both words but excludes a penalty.* That last part came from reading the passages, which is why the third call matters.

## Expected result

File names with the quoted passage for each, in seconds after the first indexing. The index stays in `/data/.groupdocs-search-index/contracts`, so a follow-up question costs two calls, not a re-read.

## The two query rules worth memorising

* **Quote phrases.** `termination penalty` without quotes finds nothing; the server returns a `hint` saying so. Use `"termination penalty"` or `termination AND penalty`.
* **`content:` before a date range.** A bare `daterange(…)` also matches the file dates the engine indexes. More: [find documents by date and amount]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}}).

The full language: [`search`]({{< ref "search/mcp/tools-reference/search.md" >}}#query-language).

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| `"documentsFound": 0` with a `hint` | an unquoted multi-word query | quote the phrase, or join the words with `AND` |
| *"Path 'x' not found in storage"* with a file list | a host path instead of a storage-relative one | pass `contracts`, not `/home/me/contracts` |
| *"The index folder … is not writable"* | Linux, container user does not own the folder | add `--user "$(id -u):$(id -g)"` |

## FAQ

**Do I need a vector database or an embedding model?** No. The index is a local full-text index; nothing calls a model or a network service.

**How big can the folder be?** In evaluation mode, 100 documents per index and 10 per result. Licensed, the limits come from disk and memory; very large corpora index once and update incrementally.

**Can the agent search several indexes?** Yes — one call per index. Name indexes by project or corpus (`contracts`, `mail`, `reports`).

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
Before reading documents in ./documents, use the groupdocs-search MCP server:
search (quote multi-word phrases) to find the files, then get_text_fragments
to quote the passages. Cite file and passage. Never read whole files to search.
```

## Honest limits

* **Evaluation mode:** an index holds at most 100 documents and a search returns at most 10 — the response says so in its `evaluation` field. [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}).
* **Full-text, not semantic:** it matches words, not meaning. Ask the agent to try variants (`OR`, `fuzzy: true`).
* **No OCR:** scans without a text layer are indexed by their file properties only.
* **Docker only:** there is no `dnx` channel for this product ([why]({{< ref "search/net/mcp/_index.md" >}})).

## Related

* [Search a folder of contracts]({{< ref "search/mcp/use-cases/search-a-folder-of-contracts.md" >}})
* [Quote matching passages]({{< ref "search/mcp/use-cases/quote-matching-passages-not-whole-files.md" >}})
* [Keep an index up to date]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}})
* [Search documents in a code repository]({{< ref "search/mcp/use-cases/search-project-documents-from-claude-code.md" >}})
* [Search email archives]({{< ref "search/mcp/use-cases/search-email-archives-locally.md" >}})
* [Find documents by date and amount]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}})
* [On-premise deployment]({{< ref "search/mcp/use-cases/on-premise-document-search.md" >}})
* [Tools reference]({{< ref "search/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}})
* Questions: [Search forum](https://forum.groupdocs.com/c/search/16)
