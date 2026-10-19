---
id: mcp-uc-keep-an-index-up-to-date
url: search/mcp/use-cases/keep-an-index-up-to-date
title: How to keep a document index current as files change
linkTitle: Keep an index current
weight: 4
description: "Keep an AI agent's document index current as files change — update_index re-reads only what changed — and use one named index per project, locally over MCP."
keywords: refresh document index AI, re-index changed files, named search index per project, keep search index current MCP, agent document memory
productName: GroupDocs.Search MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to keep a document index current as files change"
        description: "Keep an AI agent's document index current as files change — update_index re-reads only what changed — and use one named index per project, locally over MCP."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Search MCP server with Docker and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "I edited some contracts. Refresh the contracts index, then tell me which contracts mention a penalty now."
---

A GroupDocs.Search index is the agent's memory of a folder: built once, kept on disk, and brought up to date with one call. Run the server locally in Docker, and after files change ask the agent to refresh — `update_index` re-reads only the changed, added and deleted files:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

> I edited some contracts. Refresh the contracts index, then tell me which contracts mention a penalty now.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* An index built with [`add_to_index`]({{< ref "search/mcp/tools-reference/add-to-index.md" >}}) — see [search documents with AI agents]({{< ref "search/mcp/use-cases/search-documents-with-ai-agents.md" >}}).
* `/data` mounted from your host, so the index persists between container runs.

## State across sessions

```text
session 1    add_to_index --> index "contracts" --+
                                                 |  kept on disk in
session 2    search, get_text_fragments <--------+  /data/.groupdocs-search-
             (nothing is re-read)                    index/contracts
files edit   update_index --> re-reads changed, added, deleted files only
new project  create_index {"indexName":"legal-2026"} --> a separate index
```

## What the agent sees

Captured from server 26.10.1. The NDA was edited to add a penalty clause between the first and second calls:

```text
-> get_index_info {"indexName":"contracts"}
<- { "documentCount": 5, "indexedPaths": [ "contracts" ],
     "documentsWithErrors": [], "status": "Ready" }

-> update_index {"indexName":"contracts"}
<- Index 'contracts' updated: 0 added, 1 updated, 0 removed;
   5 document(s) in the index.

-> search {"query":"penalty","indexName":"contracts"}
<- documentsFound 3 — the edited initech-nda.docx now matches on its
   new clause
```

## One index per project

> Start a separate index called legal-2026 for this year's contracts.

```text
-> create_index {"indexName":"legal-2026"}
<- Index 'legal-2026' created. Add documents with add_to_index
   (indexName: 'legal-2026').
```

Names keep corpora apart — `contracts`, `mail`, `reports`, one per client — and each index lives in its own folder under `/data/.groupdocs-search-index`. [`get_index_info`]({{< ref "search/mcp/tools-reference/get-index-info.md" >}}) without a name lists them all.

## The lifecycle, and the one rule about removal

```text
create_index (optional)
      |
      v
add_to_index ---> search ---> get_text_fragments
      ^              ^
      |              |  get_index_info: counts, paths, status
update_index  (files changed on disk)
      |
remove_from_index  (only a path that was given to add_to_index)
```

The engine removes **paths that were added**, not files inside them:

```text
-> remove_from_index {"documentPath":"contracts/hooli-supply-terms.txt",
                      "indexName":"contracts"}
<- Removal failed for 'contracts/hooli-supply-terms.txt':
   'contracts/hooli-supply-terms.txt' is inside the indexed folder
   'contracts'. GroupDocs.Search removes whole indexed paths only: remove
   that folder, then add back the files or sub-folders you want with
   add_to_index. Indexed paths: 'contracts'.
```

If you expect to drop parts of a corpus — drafts, a year, a client — index them as **separate paths** (`contracts/2025`, `contracts/2026`), so each one can be removed on its own. Deleting a file on disk and calling `update_index` also removes it.

## Keeping it current without asking

* **On a schedule:** any MCP-capable automation can call `update_index` nightly; the call is cheap when little has changed.
* **In the agent's instructions:** tell it to refresh before answering (snippet below).
* **After a bulk copy:** one `update_index` picks up every added file.

## Expected result

The index reflects the folder after one call, and the counts say what changed. Indexes built by GroupDocs.Search 22.7–26.8 are converted on first use, with a `.pre-26.10-backup` copy kept next to the index.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| The index is empty after restarting the container | `/data` was not a mounted host folder | mount it: `-v /path/to/documents:/data` |
| A deleted file still matches | the index was not updated | `update_index` |
| *"…is inside the indexed folder"* | removing a file from an added folder | remove the folder, or index sub-folders separately |

## FAQ

**Does `update_index` re-read everything?** No — changed, added and deleted files in the index's paths only.

**Can two projects share documents?** Yes: add the same path to two indexes. Each index keeps its own copy of the text.

**Where does the index live if my documents are read-only?** Mount a writable folder at `/index` and set `GROUPDOCS_MCP_INDEX_PATH=/index` — [configuration]({{< ref "search/net/mcp/configuration.md" >}}).

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
This project's documents are indexed in the groupdocs-search index named
<project>. Call update_index (indexName: <project>) before answering questions
about them if files may have changed since the last session.
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
* [Search documents in a code repository]({{< ref "search/mcp/use-cases/search-project-documents-from-claude-code.md" >}})
* [Search email archives]({{< ref "search/mcp/use-cases/search-email-archives-locally.md" >}})
* [Find documents by date and amount]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}})
* [On-premise deployment]({{< ref "search/mcp/use-cases/on-premise-document-search.md" >}})
* [Tools reference]({{< ref "search/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}})
* Questions: [Search forum](https://forum.groupdocs.com/c/search/16)

## Read more

* [Project memory for AI agents: one index per project, kept current](https://blog.groupdocs.com/search/project-memory-ai-agents-document-index-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

