---
id: mcp-tools-reference
url: search/mcp/tools-reference
title: Tools reference
weight: 2
description: "Complete reference of every tool the GroupDocs.Search MCP server exposes to AI agents, with parameters, example prompts, and results."
keywords: MCP tools document search, add_to_index MCP tool, search MCP tool, get_text_fragments, full-text search tools for AI agents
productName: GroupDocs.Search MCP Server
generated: true
serverVersion: 26.10.1
toc: True
---

Complete reference of every tool the GroupDocs.Search MCP server exposes to AI agents, with parameters, example prompts, and results. Captured from a live `tools/list` call against server version **26.10.1** (raw capture: `tools-list.generated.json` in this section's source).

| Tool | What it does |
|---|---|
| [`add_to_index`]({{< ref "search/mcp/tools-reference/add-to-index.md" >}}) | Indexes a file or folder from the storage folder into a named index (created if missing) |
| [`search`]({{< ref "search/mcp/tools-reference/search.md" >}}) | Finds the documents that match a query — words, phrases, boolean, fuzzy, date and number ranges |
| [`get_text_fragments`]({{< ref "search/mcp/tools-reference/get-text-fragments.md" >}}) | Returns the matching passages with surrounding words, per document |
| [`update_index`]({{< ref "search/mcp/tools-reference/update-index.md" >}}) | Re-reads changed, added and deleted files in an index's paths |
| [`remove_from_index`]({{< ref "search/mcp/tools-reference/remove-from-index.md" >}}) | Removes a path that was added with add_to_index |
| [`get_index_info`]({{< ref "search/mcp/tools-reference/get-index-info.md" >}}) | Reports an index's documents, paths, status and the indexes that exist |
| [`create_index`]({{< ref "search/mcp/tools-reference/create-index.md" >}}) | Creates an empty named index, or empties one with recreate |
| [`get_license_status`]({{< ref "search/mcp/tools-reference/get-license-status.md" >}}) | Reports the active licensing mode and, under metered licensing, consumption |

## How the tools address documents and indexes

No tool takes a document's content. They take **paths inside the storage folder** and **index names**:

| Argument | Shape | Example |
|---|---|---|
| `documentsPath` / `documentPath` | relative to the storage folder (`/data` in Docker) — a file or a folder; omit to mean the whole folder | `"contracts"`, `"contracts/lease.pdf"` |
| `indexName` | a name, never a path; omit for the index called `default` | `"contracts"`, `"legal-2026"` |
| `query` | the search language below | `"termination AND penalty"` |

Indexes live in the index store — `/data/.groupdocs-search-index/<name>` by default, or under `GROUPDOCS_MCP_INDEX_PATH` when set — and **persist between calls and restarts**. The server never indexes its own index folder. Result paths come back relative to the storage folder (`contracts/acme-master-services-agreement.pdf`); a match inside an email or archive adds `innerPath` (the attachment's name).

## The query language

| You want | Query | Note |
|---|---|---|
| a word | `penalty` | case-insensitive unless `caseSensitive: true` |
| a phrase | `"termination penalty"` | **always quote multi-word phrases** — unquoted words do not match |
| all of / any of / not | `termination AND penalty` · `renewal OR renews` · `lease NOT draft` | also `&`, `\|`, `!`; group with parentheses |
| one unknown character | `terminat?` | matches `terminate`, not `termination` — `?` is one character |
| spelling variants | `renewl` with `fuzzy: true` | also matches near words — check the `terms` it found |
| dates written in documents | `content: daterange(2026-07-01 ~~ 2026-09-30)` | **use `content:`** — see the note below |
| numbers in a range | `content: (100000 ~~ 999999)` | whole numbers written without separators |
| a file-name word | `filename: q3` | matches words in the file name |
| a regular expression | `^inv[0-9]+` | a query starting with `^` |

There is **no trailing `*` wildcard**: use `?` patterns or `fuzzy: true` for variants. `AND` is evaluated per **document**, not per sentence — confirm with [`get_text_fragments`]({{< ref "search/mcp/tools-reference/get-text-fragments.md" >}}) that the words are about the same thing.

{{< alert style="warning" >}}
**Put `content:` in front of a date range.** The engine also indexes each file's creation and modification dates as fields, so a bare `daterange(…)` matches every file written in that period. `content: daterange(…)` searches only the dates written inside the documents. To search by file date on purpose, use `modificationdate: daterange(…)`.
{{< /alert >}}

The full grammar: [query language specification]({{< ref "search/net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}).
