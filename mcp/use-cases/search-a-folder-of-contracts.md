---
id: mcp-uc-search-a-folder-of-contracts
url: search/mcp/use-cases/search-a-folder-of-contracts
title: How to ask questions across a folder of contracts with an AI agent
linkTitle: Search contracts
weight: 2
description: "Ask an AI agent which contracts in a folder mention a clause — termination penalty, force majeure, renewal — and get the quoted clause, locally over MCP."
keywords: search contracts AI, find clause in contracts, which contracts mention, legal document search local, contract review AI agent MCP
productName: GroupDocs.Search MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to ask questions across a folder of contracts with an AI agent"
        description: "Ask an AI agent which contracts in a folder mention a clause — termination penalty, force majeure, renewal — and get the quoted clause, locally over MCP."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Search MCP server with Docker and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Which of my contracts mention a termination penalty? Quote the clause for each one."
---

Index the contracts folder once with the GroupDocs.Search MCP server, running locally in Docker, and your agent can answer *which contracts say X* with the clause quoted — PDF, Word and text contracts in the same index, nothing uploaded:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

> Which of my contracts mention a termination penalty? Quote the clause for each one.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* The server registered in your AI client, the contracts folder inside the folder mounted at `/data` — [quick start]({{< ref "search/mcp/getting-started/_index.md" >}}).

## The calls

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

## What the agent sees

Captured from server 26.10.1 against five synthetic contracts — two with a penalty, one that mentions both words but excludes it:

```text
-> search {"query":"termination penalty","indexName":"contracts"}
<- { "documentsFound": 0,
     "hint": "Unquoted words separated by spaces do not match. Search the
              phrase as \"termination penalty\", or combine the words with
              AND / OR, e.g. termination AND penalty." }

-> search {"query":"termination AND penalty","indexName":"contracts"}
<- documentsFound 3: acme-master-services-agreement.pdf,
   globex-consulting-agreement.docx, initech-nda.docx

-> get_text_fragments {"query":"\"termination penalty\"",
                       "indexName":"contracts"}
<- 2 document(s) match.
   === contracts/acme-master-services-agreement.pdf ===
     …a termination penalty of three monthly fees applies, payable
     within 30…
   === contracts/globex-consulting-agreement.docx ===
     …Early termination penalty: the Client pays 50 percent of the fees…
```

Three things to notice:

* **The first query missed** — unquoted words do not match — and the `hint` told the agent how to fix it. Agents usually correct themselves on the next call.
* **`AND` found three files, the phrase found two.** `AND` is evaluated per document: the Initech NDA contains both words in the sentence *"There is no fee and no penalty associated with termination"*. The fragments call is what tells a match from a mention.
* **One index, three formats** — PDF, DOCX and TXT answer the same query.

## More questions that work

> Which contracts have a force majeure clause?

`"force majeure"` → the Acme agreement only.

> Which contracts can the client terminate for convenience, and with how much notice?

`terminat? AND notice` (the `?` matches `terminate`), then fragments with `termsAfter: 20` to read the notice period.

> Which leases renew automatically?

`renewal OR renews` → the Umbrella lease; the passage says *"renews automatically for 12 months unless notice is given"*.

## Expected result

A list of contract files with the quoted clause under each — and, when you ask for it, the contracts that mention the words without the clause. Answers arrive in seconds after the first indexing.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| A clause you know is there is not found | the contract is a scan — no text layer | OCR it first; the index has its file properties only |
| A contract matches but does not contain the clause | `AND` matched the words in different places | read the fragments; search the exact phrase in quotes |
| *"…is inside the indexed folder 'contracts'"* when removing one file | removal works on added paths | remove `contracts` and add back what you want — [keep an index current]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}}) |

## FAQ

**Is this legal advice?** No — it finds and quotes text. The agent's summary is only as good as the passage it read; check the quote against the contract.

**Does it handle 300 contracts?** Licensed, yes: index once, and each question is two calls. Evaluation mode stops at 100 documents per index and 10 results per search.

**Can I search contract metadata, such as the date signed?** Dates written in the text: `content: daterange(2026-01-01 ~~ 2026-06-30)`. File dates: `modificationdate: daterange(…)`.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
For questions about contracts in ./documents/contracts, use the groupdocs-search
MCP server: search with quoted phrases, then get_text_fragments to quote the
clause. Report contracts that mention the words without the clause separately.
```

## Honest limits

* **Evaluation mode:** an index holds at most 100 documents and a search returns at most 10 — the response says so in its `evaluation` field. [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}).
* **Full-text, not semantic:** it matches words, not meaning. Ask the agent to try variants (`OR`, `fuzzy: true`).
* **No OCR:** scans without a text layer are indexed by their file properties only.
* **Docker only:** there is no `dnx` channel for this product ([why]({{< ref "search/net/mcp/_index.md" >}})).

## Related

* [Search documents with AI agents]({{< ref "search/mcp/use-cases/search-documents-with-ai-agents.md" >}})
* [Quote matching passages]({{< ref "search/mcp/use-cases/quote-matching-passages-not-whole-files.md" >}})
* [Keep an index up to date]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}})
* [Search documents in a code repository]({{< ref "search/mcp/use-cases/search-project-documents-from-claude-code.md" >}})
* [Search email archives]({{< ref "search/mcp/use-cases/search-email-archives-locally.md" >}})
* [Find documents by date and amount]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}})
* [On-premise deployment]({{< ref "search/mcp/use-cases/on-premise-document-search.md" >}})
* [Tools reference]({{< ref "search/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}})
* Questions: [Search forum](https://forum.groupdocs.com/c/search/16)
