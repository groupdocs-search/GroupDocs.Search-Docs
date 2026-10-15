---
id: mcp-uc-find-documents-by-date-and-amount
url: search/mcp/use-cases/find-documents-by-date-and-amount
title: How to find documents by date range, amount and file name with an AI agent
linkTitle: Find by date and amount
weight: 7
description: "Find documents that mention a date in a range, an amount in a range, or a word in the file name with an AI agent, locally over MCP — and avoid matching file dates by accident."
keywords: search documents by date range AI, find reports amount over, date range search MCP, numeric range search documents, filename search agent
productName: GroupDocs.Search MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to find documents by date range, amount and file name with an AI agent"
        description: "Find documents that mention a date in a range, an amount in a range, or a word in the file name with an AI agent, locally over MCP — and avoid matching file dates by accident."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Search MCP server with Docker and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Which reports mention a date in Q3 2026, and which mention an amount over 100000?"
---

The GroupDocs.Search MCP server finds documents by **dates written in them**, by **numbers in a range**, and by **words in the file name** — locally, across Word, Excel, PDF and email. The one rule: put `content:` in front of a date range, or it also matches the files' own dates:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

> Which reports mention a date in Q3 2026, and which mention an amount over 100000?

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* The reports indexed with [`add_to_index`]({{< ref "search/mcp/tools-reference/add-to-index.md" >}}).
* Dates written as `yyyy-MM-dd`, `MM/dd/yyyy` or `dd.MM.yyyy` (the engine's defaults); numbers written without separators (`125000`, not `125,000`).

## The trap, and the fix

Captured from server 26.10.1 against four reports; only the board minutes mention an October date:

```text
-> search {"query":"daterange(2026-10-01 ~~ 2026-10-31)",
          "indexName":"reports"}
<- documentsFound 4 — every report: the engine also indexes each file's
   creation and modification dates, and the files were written in October

-> search {"query":"content: daterange(2026-10-01 ~~ 2026-10-31)",
          "indexName":"reports"}
<- documentsFound 1: reports/board-minutes-2026-10.pdf
```

`content:` restricts the range to the documents' own text. To search by file date on purpose, name that field: `modificationdate: daterange(2026-10-01 ~~ 2026-10-31)`.

## Dates, amounts, names

```text
-> search {"query":"content: daterange(2026-07-01 ~~ 2026-09-30)", ...}
<- documentsFound 2: capex-register-2026.xlsx, q3-operations-report.docx

-> search {"query":"content: (100000 ~~ 999999)", ...}
<- documentsFound 2:
   capex-register-2026.xlsx — terms ["230000", "410000"]
   q3-operations-report.docx — terms ["125000"]

-> search {"query":"filename: q3", ...}
<- documentsFound 1: reports/q3-operations-report.docx

-> search {"query":"warehouse AND content: (100000 ~~ 999999)", ...}
<- documentsFound 2: capex-register-2026.xlsx, q3-operations-report.docx
```

Spreadsheet cells count: the XLSX matched on its date and amount columns.

## Combine with care

`AND` is evaluated per **document**. The last query matched the capex register because it contains *Warehouse racking* and also *410000* — on the solar-roof row. Ask for the fragments when the combination matters:

> For each match, quote the line with the amount.

[`get_text_fragments`]({{< ref "search/mcp/tools-reference/get-text-fragments.md" >}}) with the same query shows which line each number is on.

## Expected result

The documents with a date or amount in range, plus the passage for each when you ask — from Word, Excel, PDF and email in one index.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| A date range matches almost every file | no `content:` prefix — file dates matched | `content: daterange(…)` |
| An amount is not found | written with separators (`125,000`, `125 000`) | search the parts, or a word next to it; ranges see whole numbers only |
| A date is not found | a format outside the defaults (`15 Aug 2026`) | search the words (`August AND 2026`) |

## FAQ

**Does the range understand currencies?** No — numbers are numbers. `100000 ~~ 999999` matches any whole number in that range, whatever it counts.

**Can it search by the date a file was modified?** Yes: `modificationdate: daterange(…)`; also `creationdate:`.

**Are Excel dates found?** Dates stored as text in cells, yes — as in the capex register above.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
For date questions about documents in ./documents, search with
content: daterange(yyyy-MM-dd ~~ yyyy-MM-dd) — never a bare daterange, which also
matches file dates. Use content: (min ~~ max) for amounts; quote the matching line.
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
* [Search documents in a code repository]({{< ref "search/mcp/use-cases/search-project-documents-from-claude-code.md" >}})
* [Search email archives]({{< ref "search/mcp/use-cases/search-email-archives-locally.md" >}})
* [On-premise deployment]({{< ref "search/mcp/use-cases/on-premise-document-search.md" >}})
* [Tools reference]({{< ref "search/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}})
* Questions: [Search forum](https://forum.groupdocs.com/c/search/16)
