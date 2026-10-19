---
id: mcp-uc-search-email-archives-locally
url: search/mcp/use-cases/search-email-archives-locally
title: How to search email archives and attachments with an AI agent, locally
linkTitle: Search email archives
weight: 6
description: "Search Outlook archives and email files — PST, OST, MSG, EML — and the documents attached to them with an AI agent, locally over MCP, without exporting anything."
keywords: search PST with AI, search Outlook archive locally, EML MSG search agent, search email attachments AI, e-discovery local MCP
productName: GroupDocs.Search MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to search email archives and attachments with an AI agent, locally"
        description: "Search Outlook archives and email files — PST, OST, MSG, EML — and the documents attached to them with an AI agent, locally over MCP, without exporting anything."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Search MCP server with Docker and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Index the mail folder. Which message mentions a price increase, and what exactly does it say?"
---

The GroupDocs.Search MCP server opens email files and archives — PST, OST, MSG, EML — and indexes each message **and its attachments**, locally in Docker. Ask the agent a question, and a match inside an attached document comes back with the message file and the attachment's name:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

> Index the mail folder. Which message mentions a price increase, and what exactly does it say?

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* The archive or email files inside the folder mounted at `/data` — export nothing, convert nothing.
* For archives larger than a few hundred messages, a [license]({{< ref "search/mcp/getting-started/licensing.md" >}}): evaluation mode stops at 100 documents per index, and every message and attachment counts.

## How a match inside an attachment is reported

```text
mail/2026-09-18-re-q3-pricing.eml          <- filePath
  |- body: "please find the updated quotation attached"
  '- attachment: quotation-Q-2026-118.docx  <- innerPath
        "...we announce a price increase of 6 percent..."

search {"query":"\"price increase\"","indexName":"mail"}
  -> {"filePath":"mail/2026-09-18-re-q3-pricing.eml",
      "innerPath":"quotation-Q-2026-118.docx", "occurrences":1}
```

## What the agent sees

Captured from server 26.10.1 with three EML messages; the phrase occurs only inside the Word quotation attached to one of them:

```text
-> add_to_index {"documentsPath":"mail","indexName":"mail"}
<- Indexed 'mail' into index 'mail': 3 document(s) added, 3 in the index.

-> search {"query":"\"price increase\"","indexName":"mail"}
<- { "documentsFound": 1, "documents": [
     { "filePath": "mail/2026-09-18-re-q3-pricing.eml",
       "innerPath": "quotation-Q-2026-118.docx",
       "occurrences": 1 } ] }

-> get_text_fragments {"query":"\"price increase\"","indexName":"mail"}
<- === mail/2026-09-18-re-q3-pricing.eml → quotation-Q-2026-118.docx
   (1 occurrence(s)) ===
     [content] …Due to raw material costs we announce a price increase
   of 6 percent from 2026-11-01.…

-> search {"query":"invoice AND 1042","indexName":"mail"}
<- documentsFound 1: mail/2026-09-22-invoice-1042.eml (6 occurrences)
```

The message body says only *"please find the updated quotation attached"*; the answer was in the attachment, and `innerPath` says which one.

## Questions that work on mail

> Which message from Fabrikam mentions a delivery delay?

`fabrikam AND delay` → the delivery-delay message. Sender addresses are indexed with the message: `contoso` also finds the invoice whose body never names the company.

> Find every email that mentions order 77812.

`77812` — whole numbers are words.

> Which invoices fall due in October?

`invoice AND content: daterange(2026-10-01 ~~ 2026-10-31)` → invoice 1042, *"due on 2026-10-22"*. It matches dates written in the message text — the `Date:` header uses a format the range does not read; see the [date-range note]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}}).

## Expected result

The message file, the attachment where relevant, and the quoted passage — from an archive that never left the machine and was never converted.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Results stop at 10, or the index at 100 | evaluation mode — every message and attachment counts | license the server |
| A large PST takes a long time to index | every message and attachment is opened | index once, then `update_index`; split by folder if you need progress |
| An attachment's text is missing | the attachment is a scan, or a format without text | OCR outside the server; images are indexed by properties only |

## FAQ

**Does it need Outlook installed?** No. The engine reads PST, OST, MSG and EML directly.

**Is anything uploaded?** No. The archive is read inside the local container; only what the agent quotes back reaches your model provider.

**Can it search the archive and the documents folder together?** Index them into one index, or keep `mail` and `documents` as separate indexes and ask the agent to search both.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
Email under ./documents/mail is indexed in the groupdocs-search index 'mail'.
For questions about correspondence, search that index; report filePath and innerPath
(the attachment) for every match, and quote with get_text_fragments.
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
* [Find documents by date and amount]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}})
* [On-premise deployment]({{< ref "search/mcp/use-cases/on-premise-document-search.md" >}})
* [Tools reference]({{< ref "search/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}})
* Questions: [Search forum](https://forum.groupdocs.com/c/search/16)

## Read more

* [Search your email archive with an AI agent: PST, MSG, EML and the attachments inside](https://blog.groupdocs.com/search/search-email-archive-pst-attachments-ai-agent-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

