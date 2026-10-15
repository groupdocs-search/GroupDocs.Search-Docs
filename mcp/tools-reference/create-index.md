---
id: mcp-tool-create-index
url: search/mcp/tools-reference/create-index
title: create_index
weight: 7
description: "The create_index MCP tool creates an empty named document index, or empties an existing one, so each project can keep its own searchable memory."
keywords: create_index MCP tool, named search index per project, recreate document index
productName: GroupDocs.Search MCP Server
generated: true
serverVersion: 26.10.1
toc: True
---

`create_index` creates an empty **named** index — one per project, client or corpus — or, with `recreate: true`, empties an existing one. You rarely need it: `add_to_index` creates indexes on demand. Example prompt: *"Start a new index called legal-2026."*

**Tool description (as the AI agent sees it):**

> Creates a named full-text search index, or opens it when it already exists, and reports what it holds. An index is a persistent, searchable copy of the text of your documents; it survives server restarts and lives in the server's index folder, never next to the documents. You rarely need to call this first — add_to_index creates the index on demand. Call it when the user wants a separate index for a set of documents ('make an index called contracts'), to check whether an index exists, or with recreate=true to wipe an index and start over. Indexes written by GroupDocs.Search 22.7–26.8 are converted to the current format automatically and the original is kept. Pass index names such as 'contracts' or 'legal/2026', never filesystem paths. Returns one or two sentences: whether the index was created or opened, and its document count and indexed paths (relative to the storage folder). In evaluation mode an index holds at most 100 documents. On failure the response starts with 'Index creation failed for' followed by the exception type and message, and isError is set.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `indexName` | string | no | Name of the index, for example 'contracts'. Omit to use the default index. |
| `recreate` | boolean | no | Delete the index's existing content and start empty. Default false. |

## Example call

```json
{
  "name": "create_index",
  "arguments": {
    "indexName": "legal-2026"
  }
}
```

## Result

A confirmation line — captured from server 26.10.1:

```text
Index 'legal-2026' created. Add documents with add_to_index
(indexName: 'legal-2026').
```

`recreate: true` deletes the index's content first — use it after licensing the server to rebuild without the evaluation text.

## Example prompts

* *"Create an index called legal-2026."*
* *"Empty the contracts index and start over."*
