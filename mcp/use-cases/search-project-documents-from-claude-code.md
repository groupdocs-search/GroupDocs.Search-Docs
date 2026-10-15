---
id: mcp-uc-search-project-documents-from-claude-code
url: search/mcp/use-cases/search-project-documents-from-claude-code
title: How to let Claude Code or Cursor search the PDFs and Word documents in a repository
linkTitle: Search repo documents
weight: 5
description: "Give Claude Code, Cursor or Copilot full-text search over the PDF and Word documents in a code repository — datasheets, specs, decision records — locally over MCP."
keywords: Claude Code search PDF, Cursor read docx in repo, coding agent documents index, MCP server for repo documents, search specs datasheets AI
productName: GroupDocs.Search MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to let Claude Code or Cursor search the PDFs and Word documents in a repository"
        description: "Give Claude Code, Cursor or Copilot full-text search over the PDF and Word documents in a code repository — datasheets, specs, decision records — locally over MCP."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Search MCP server with Docker and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Index the docs folder, then tell me what the sensor datasheet says about the operating temperature and which ADR decided the telemetry storage."
---

Coding agents search a repository with grep, and grep cannot read inside a PDF datasheet or a Word specification — the text is compressed. Register the GroupDocs.Search MCP server locally in Claude Code or Cursor, index the repository's `docs/` folder, and the agent can search and quote those documents like code:

```bash
claude mcp add groupdocs-search -- docker run --rm -i \
  -v "$(pwd)":/data ghcr.io/groupdocs-search/search-net-mcp:latest
```

> Index the docs folder, then tell me what the sensor datasheet says about the operating temperature and which ADR decided the telemetry storage.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* Docker; run the command above from the repository root, so the repository is `/data`. On Linux add `--user "$(id -u):$(id -g)"` after `-i`.
* Cursor and VS Code: the same `docker run` arguments in `.cursor/mcp.json` or `.vscode/mcp.json` — [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}).

## What grep sees, and what the index sees

In a test repository with a PDF datasheet and two Word documents, `grep -ri "TimescaleDB\|operating temperature\|I2C" docs/` finds **nothing** — PDF and DOCX store their text compressed. The only hit is a code comment pointing at the datasheet. The same questions through the index, captured from server 26.10.1:

```text
-> add_to_index {"documentsPath":"docs","indexName":"gateway"}
<- Indexed 'docs' into index 'gateway': 3 document(s) added,
   3 in the index.

-> search {"query":"TimescaleDB","indexName":"gateway"}
<- documentsFound 1: docs/adr-007-telemetry-storage.docx

-> get_text_fragments {"query":"I2C","indexName":"gateway"}
<- 2 document(s) match.
   === docs/requirements-v2.docx (1 occurrence(s)) ===
     [content] …REQ-15: the I2C bus runs at 400 kHz.…
   === docs/tmp117-sensor-datasheet.pdf (1 occurrence(s)) ===
     [content] …I2C address selection: ADD0 pin to GND gives 0x48…
```

(Paths shown relative to the repository root, as they are when the repository is mounted at `/data`.)

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

The loop is the same as for any folder; what changes is that the agent now has a retrieval tool for the documents next to the code — the datasheet behind a constant, the ADR behind a design choice, the requirement behind a test.

## Keep the index out of git

The index is written to `.groupdocs-search-index/` at the root of the mounted folder. Add it to `.gitignore`:

```text
.groupdocs-search-index/
```

After pulling changes, ask for an `update_index` — it re-reads only what changed ([keep an index current]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}})).

## Expected result

Answers that cite `docs/…pdf` and `docs/…docx` passages alongside the code the agent already reads — *"the datasheet gives an operating range of -55 C to 150 C; ADR-007 chose TimescaleDB with 13-month retention."*

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| The agent greps instead of searching | it does not know when to use the tool | add the instruction below to `CLAUDE.md` / `AGENTS.md` |
| `git status` shows `.groupdocs-search-index/` | the index lives in the mounted folder | add it to `.gitignore` |
| *"The index folder … is not writable"* | Linux, container user does not own the repository | add `--user "$(id -u):$(id -g)"` |

## FAQ

**Why not let the agent convert the PDFs to text first?** It can, file by file, every time. The index does it once, keeps it current with one call, and answers *which* file before reading any.

**Does it index the source code too?** It indexes what you point it at. Code is better searched with the agent's own tools; point `add_to_index` at `docs/` (or wherever the documents are).

**Will it slow the agent down?** The first indexing takes a moment. On our small test index, searches returned in 80–240 ms, and the first call of a session in about a second.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
PDF, DOCX and XLSX files under docs/ are indexed in the groupdocs-search MCP index
'docs'. grep cannot read them: use search (quote phrases) and get_text_fragments
for anything in specs, datasheets or ADRs, and cite the file.
Run update_index (indexName: docs) after pulling changes.
```

## Honest limits

* **Evaluation mode:** an index holds at most 100 documents and a search returns at most 10 — the response says so in its `evaluation` field. [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}).
* **Full-text, not semantic:** it matches words, not meaning. Ask the agent to try variants (`OR`, `fuzzy: true`).
* **No OCR:** scans without a text layer are indexed by their file properties only.
* **Docker only:** there is no `dnx` channel for this product ([why]({{< ref "search/net/mcp/_index.md" >}})).

## Related

* [Search documents with AI agents]({{< ref "search/mcp/use-cases/search-documents-with-ai-agents.md" >}})
* [Search a folder of contracts]({{< ref "search/mcp/use-cases/search-a-folder-of-contracts.md" >}})
* [Quote matching passages]({{< ref "search/mcp/use-cases/quote-matching-passages-not-whole-files.md" >}})
* [Keep an index up to date]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}})
* [Search email archives]({{< ref "search/mcp/use-cases/search-email-archives-locally.md" >}})
* [Find documents by date and amount]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}})
* [On-premise deployment]({{< ref "search/mcp/use-cases/on-premise-document-search.md" >}})
* [Tools reference]({{< ref "search/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}})
* Questions: [Search forum](https://forum.groupdocs.com/c/search/16)
