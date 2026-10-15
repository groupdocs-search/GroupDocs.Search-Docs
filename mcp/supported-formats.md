---
id: mcp-supported-formats
url: search/mcp/supported-formats
title: Supported formats
weight: 4
description: "The GroupDocs.Search MCP server indexes 82 formats — PDF, Word, Excel, PowerPoint, OpenDocument, email archives (PST, OST, MSG, EML) with their attachments, HTML, e-books and more — for AI agents to search locally."
keywords: MCP server supported formats, search PST with AI, index PDF Word Excel MCP, email archive search formats, document search formats
productName: GroupDocs.Search MCP Server
toc: True
---

The server indexes **82 formats** — the list below is what [`get_index_info`]({{< ref "search/mcp/tools-reference/get-index-info.md" >}}) returns with `includeSupportedFormats: true` on server 26.10.1. The library's canonical matrix: [supported document formats]({{< ref "search/net/getting-started/supported-document-formats.md" >}}).

| Family | Formats | What is searchable |
|---|---|---|
| PDF | PDF | the text layer, plus file properties |
| Word processing | DOC, DOCX, DOCM, DOT, DOTX, DOTM, RTF, ODT, OTT, TXT, LOG, MD | text and properties |
| Spreadsheets | XLS, XLSX, XLSB, XLSM, XLT, XLTX, XLTM, XLA, XLAM, ODS, OTS, CSV, TSV | cell text, numbers and dates |
| Presentations | PPT, PPTX, PPTM, POT, POTX, POTM, PPS, PPSX, PPSM, ODP | slide text |
| Email | EML, EMLX, MSG, **PST, OST**, MHT, MHTML | message text **and attachments** — a match inside an attachment returns `innerPath` |
| Web and e-books | HTM, HTML, XHTML, EPUB, FB2, CHM | text |
| Other documents | ONE, MPP, VSD, VSS, DJVU, DJV, TORRENT | text and properties where the format has them |
| Images | BMP, GIF, JPG, JPEG, JPE, JP2, PNG, TIF, TIFF, WEBP, PSD, EMF, WMF, DCM, DIC, DICOM, DC3 | file properties only — **no OCR** |
| Audio and video | ASF, AVI, FLV, MOV, QT, MP3, WAV | file properties only |

**Three rules that explain most results**

* **A text layer or nothing.** Born-digital documents are searchable by their words; a scanned PDF without a text layer is not, because there is no OCR step.
* **Containers are opened.** Emails, PST/OST archives and their attachments are indexed item by item, so *"which message says X"* works across an archive — see [search email archives]({{< ref "search/mcp/use-cases/search-email-archives-locally.md" >}}).
* **File properties are indexed too.** Each file's creation and modification dates are searchable fields — useful on purpose (`modificationdate: daterange(…)`), surprising by accident (a bare `daterange(…)` matches them). Put `content:` in front of a date range to search the documents' own dates.
