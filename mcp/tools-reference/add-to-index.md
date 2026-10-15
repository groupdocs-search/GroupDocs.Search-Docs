---
id: mcp-tool-add-to-index
url: search/mcp/tools-reference/add-to-index
title: add_to_index
weight: 1
description: "The add_to_index MCP tool indexes a file or folder from the storage folder into a named full-text index, locally — PDF, Office, email and 80+ formats."
keywords: add_to_index MCP tool, index folder for AI agent, full-text index documents locally, index PDF Word email MCP
productName: GroupDocs.Search MCP Server
generated: true
serverVersion: 26.10.1
toc: True
---

`add_to_index` reads a file or a folder from the storage folder and adds its text to a **named index**, creating the index if it does not exist. Indexing is the one slow step; every search afterwards reads the index, not the documents. Example prompt: *"Index the contracts folder."*

**Tool description (as the AI agent sees it):**

> Adds documents to a full-text search index so they can be searched: a single file, a folder (indexed recursively), or the whole storage folder. Indexes PDF, DOCX, DOC, XLSX, XLS, PPTX, PPT, ODT, ODS, ODP, RTF, TXT, HTML, EPUB, FB2, EML, EMLX, MSG, PST, OST, ONE, ZIP and 70+ more formats, including documents inside archives and email attachments. Call this immediately whenever the user asks to index documents, make files searchable, or search documents that have not been indexed yet — the index is created on demand, so create_index is not required first. Do NOT pre-check whether the path exists; pass what the user said. documentsPath is relative to the storage folder ('contracts', 'reports/2026/q3.pdf'); omit it to index everything in storage. Adding a path that is already indexed re-reads only what changed. Large folders take time: indexing reads every document. Returns a short report: documents added in this call, total documents in the index, the index name, and any files the engine could not read. In evaluation mode an index holds at most 100 documents; indexing stops at that count. On failure the response starts with 'Indexing failed for' followed by the exception type and message, and isError is set; a missing path lists the files and folders that do exist.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `documentsPath` | string | no | File or folder to index, relative to the storage folder, e.g. 'contracts' or 'contracts/lease.pdf'. Omit to index the whole storage folder. |
| `indexName` | string | no | Name of the index to add to, e.g. 'contracts'. Created if it does not exist. Omit to use the default index. |

## Example call

```json
{
  "name": "add_to_index",
  "arguments": {
    "documentsPath": "contracts",
    "indexName": "contracts"
  }
}
```

## Result

A line with the counts — captured from server 26.10.1:

```text
Indexed 'contracts' into index 'contracts': 5 document(s) added, 5 in the index.
```

In evaluation mode the response ends with the evaluation note (*"Evaluation mode: an index holds at most 100 documents and a …"*). Indexing a folder again adds only what is new; to re-read **changed** files, use [`update_index`]({{< ref "search/mcp/tools-reference/update-index.md" >}}).

Indexes built by GroupDocs.Search 22.7–26.8 are converted on first use (a backup is kept in a `.pre-26.10-backup` folder next to the index) and the response says so.

On failure the text starts with `Indexing failed for` followed by the reason, and `isError` is set. An unknown path answers with the folder's contents; an index folder the server cannot write answers with the fix (`--user` on Linux, or a writable `/index` volume).

## Example prompts

* *"Index the contracts folder."*
* *"Add the mail folder to an index called mail."*
* *"Index everything in my documents folder."*
* *"Add reports/2026 to the reports index."*

See it used end-to-end: [Search a folder of contracts]({{< ref "search/mcp/use-cases/search-a-folder-of-contracts.md" >}}).
