---
id: mcp-tool-remove-from-index
url: search/mcp/tools-reference/remove-from-index
title: remove_from_index
weight: 5
description: "The remove_from_index MCP tool removes a file or folder that was added to an index; it explains when a path is inside an indexed folder instead."
keywords: remove_from_index MCP tool, remove folder from search index, delete documents from index AI
productName: GroupDocs.Search MCP Server
generated: true
serverVersion: 26.10.1
toc: True
---

`remove_from_index` removes a **path that was added** with [`add_to_index`]({{< ref "search/mcp/tools-reference/add-to-index.md" >}}) — a file or a folder — and everything under it. Example prompt: *"Remove the drafts folder from the index."*

**Tool description (as the AI agent sees it):**

> Removes an indexed path — a folder or file that was passed to add_to_index — from a search index, so its documents stop appearing in search results. The files on disk are not touched. Call this when the user wants documents excluded from search, e.g. 'stop searching the drafts folder' or 'remove old-contract.pdf from the index'. documentPath is relative to the storage folder and must be one of the index's indexed paths (get_index_info lists them). The engine removes whole indexed paths only: a single file inside an indexed folder cannot be removed on its own — remove the folder, then add back the files or sub-folders you want with add_to_index. Returns a sentence with the number of documents removed and the total left in the index. On failure the response starts with 'Removal failed for' followed by the reason, and isError is set; a path that is not an indexed path lists the ones that are.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `documentPath` | string | yes | File or folder to remove, relative to the storage folder, e.g. 'drafts' or 'contracts/old-contract.pdf'. |
| `indexName` | string | no | Name of the index, e.g. 'contracts'. Omit to use the default index. |

## Example call

```json
{
  "name": "remove_from_index",
  "arguments": {
    "documentPath": "drafts",
    "indexName": "contracts"
  }
}
```

## Result

A line with the number of documents removed and left — `Removed 'drafts' from index 'contracts': <n> document(s) removed, <m> left.`

The engine removes whole **indexed paths**. A file inside an indexed folder is not one, and the server says so rather than failing silently — captured from server 26.10.1:

```text
Removal failed for 'contracts/hooli-supply-terms.txt':
'contracts/hooli-supply-terms.txt' is inside the indexed folder 'contracts'.
GroupDocs.Search removes whole indexed paths only: remove that folder, then add
back the files or sub-folders you want with add_to_index.
Indexed paths: 'contracts'.
```

(Line breaks added for width; the response is one paragraph.) If you expect to remove parts of a corpus, index those parts as separate paths.

## Example prompts

* *"Remove the drafts folder from the contracts index."*
* *"Take reports/2024 out of the reports index."*

See it used end-to-end: [Keep an index up to date]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}}).
