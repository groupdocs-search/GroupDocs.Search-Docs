---
id: mcp-tool-get-index-info
url: search/mcp/tools-reference/get-index-info
title: get_index_info
weight: 6
description: "The get_index_info MCP tool reports an index's document count, indexed paths, status and the indexes that exist — and, on request, every file format the engine can index."
keywords: get_index_info MCP, list search indexes, index status MCP, supported formats search index
productName: GroupDocs.Search MCP Server
generated: true
serverVersion: 26.10.1
toc: True
---

`get_index_info` reports what an index holds — document count, the paths it covers, files that failed, its status — and which other indexes exist. With `includeSupportedFormats: true` it also lists the formats the engine indexes. Example prompt: *"What is in the contracts index?"*

**Tool description (as the AI agent sees it):**

> Describes a search index without changing it: whether it exists, how many documents it holds, which paths are indexed, which documents failed to index, its format version, and the other indexes available. Call this when the user asks what is indexed, how big an index is, which indexes exist, or before searching to confirm the right documents are in the index. Safe to call with no arguments: it reports on the default index even when that index does not exist yet. Also lists the document formats the engine can index when includeSupportedFormats is true. Returns JSON: `index`, `exists`, `documentCount`, `indexedPaths` (relative to the storage folder), `documentsWithErrors`, `version`, `segmentCount`, `termCount`, `status`, `availableIndexes`, optional `supportedFormats` and `evaluation`. On failure the response starts with 'Index-info lookup failed for' followed by the exception type and message, and isError is set.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `indexName` | string | no | Name of the index, e.g. 'contracts'. Omit to use the default index. |
| `includeSupportedFormats` | boolean | no | Also list the file formats the engine can index. Default false. |

## Example call

```json
{
  "name": "get_index_info",
  "arguments": {
    "indexName": "contracts"
  }
}
```

## Result

JSON — captured from server 26.10.1 (evaluation note trimmed):

```json
{
  "index": "contracts",
  "exists": true,
  "documentCount": 5,
  "indexedPaths": [ "contracts" ],
  "documentsWithErrors": [],
  "version": "26.10",
  "segmentCount": 1,
  "termCount": 158,
  "status": "Ready",
  "availableIndexes": [ "contracts" ]
}
```

For an index that does not exist yet, `exists` is `false` and a `hint` says to call `add_to_index`. `indexedPaths` is the list that [`remove_from_index`]({{< ref "search/mcp/tools-reference/remove-from-index.md" >}}) accepts.

## Example prompts

* *"What is in the contracts index?"*
* *"Which indexes do I have?"*
* *"Can you index .pst files? List the supported formats."*
