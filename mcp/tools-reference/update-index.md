---
id: mcp-tool-update-index
url: search/mcp/tools-reference/update-index
title: update_index
weight: 4
description: "The update_index MCP tool refreshes a document index after files change — it re-reads changed, added and deleted files in the index's paths only."
keywords: update_index MCP tool, refresh document index AI, re-index changed files, keep search index current
productName: GroupDocs.Search MCP Server
generated: true
serverVersion: 26.10.1
toc: True
---

`update_index` brings an index up to date with the files on disk — **changed, added and deleted** files in the paths the index covers — without re-reading everything. Example prompt: *"I edited some contracts; refresh the index."*

**Tool description (as the AI agent sees it):**

> Brings a search index up to date with the files on disk: re-reads documents that changed, adds new files that appeared inside already-indexed folders, and drops documents whose files were deleted. Call this when the user says files were edited, added or removed, asks to 'refresh' or 're-index', or before searching an index that may be stale. It only looks at paths already in the index; to index a new folder use add_to_index. Returns a short report: documents added, updated and removed, and the total now in the index. In evaluation mode new files are only added while the index holds fewer than 100 documents. On failure the response starts with 'Index update failed for' followed by the exception type and message, and isError is set.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `indexName` | string | no | Name of the index to update, e.g. 'contracts'. Omit to use the default index. |

## Example call

```json
{
  "name": "update_index",
  "arguments": {
    "indexName": "contracts"
  }
}
```

## Result

A line with the counts — captured from server 26.10.1 after one contract was edited:

```text
Index 'contracts' updated: 0 added, 1 updated, 0 removed;
5 document(s) in the index.
```

(Wrapped for width; the response is one line.)

On failure the text starts with `Index update failed for`, and `isError` is set; an unknown index lists the indexes that exist.

## Example prompts

* *"Refresh the contracts index."*
* *"I added files to the mail folder — update the mail index."*
* *"Update every index."*

See it used end-to-end: [Keep an index up to date]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}}).
