---
id: mcp
url: search/mcp
title: GroupDocs.Search MCP Server
weight: 6
description: "GroupDocs.Search MCP server lets AI agents like Claude, Cursor, and Copilot index a folder of documents — PDF, Word, Excel, PowerPoint, email archives and more — and search it by content, locally on your machine, then quote the matching passages."
keywords: document search MCP server, full-text search for AI agents, search local documents with Claude, index folder MCP, search PDF Word email locally
productName: GroupDocs.Search MCP Server
hideChildren: True
toc: True
---

**GroupDocs.Search MCP server** lets AI agents like Claude, Cursor, and Copilot **index a folder of documents and search it by content** — PDF, Word, Excel, PowerPoint, email archives with their attachments, and 80+ formats in all — **locally on your machine**, then quote the passages that match. Nothing is uploaded, and the index stays on disk for the next question. Run it with one command:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

On **Linux**, add `--user "$(id -u):$(id -g)"` before the image name — the container runs as a non-root user, and without it the index cannot be written into a folder you own ([why]({{< ref "search/net/mcp/linux-installation.md" >}}#run-the-container-as-your-user)).

{{< alert style="info" >}}
**This product ships via Docker only.** The packed .NET tool is 280.9 MiB — the GroupDocs.Search engine is about 200 MiB with its ONNX and SkiaSharp natives — which is over nuget.org's 250 MB package limit, so this product ships **via Docker only**, and there is no `dnx` command for it. Every command in these pages reflects that. Details: [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}).
{{< /alert >}}

Then ask your agent, in plain language:

> Index the contracts folder, then tell me which contracts mention a termination penalty and quote the clause.

## What you can do

Eight tools (full details in the [tools reference]({{< ref "search/mcp/tools-reference/_index.md" >}})):

| The user asks… | The agent calls | It gets back |
|---|---|---|
| "Index this folder" | [`add_to_index`]({{< ref "search/mcp/tools-reference/add-to-index.md" >}}) | documents added, documents in the index |
| "Which files mention X?" | [`search`]({{< ref "search/mcp/tools-reference/search.md" >}}) | matching files as JSON — relevance, occurrences, terms; `innerPath` for attachments |
| "What does it say about X?" | [`get_text_fragments`]({{< ref "search/mcp/tools-reference/get-text-fragments.md" >}}) | the passages, with words of context, per file |
| "Files changed — refresh" | [`update_index`]({{< ref "search/mcp/tools-reference/update-index.md" >}}) | added / updated / removed counts |
| "Forget the drafts folder" | [`remove_from_index`]({{< ref "search/mcp/tools-reference/remove-from-index.md" >}}) | documents removed and left |
| "What's in the index? Can it read .pst?" | [`get_index_info`]({{< ref "search/mcp/tools-reference/get-index-info.md" >}}) | counts, paths, status; the format list |
| "Start a separate index for project Y" | [`create_index`]({{< ref "search/mcp/tools-reference/create-index.md" >}}) | an empty named index |
| "Why only 10 results?" | [`get_license_status`]({{< ref "search/mcp/tools-reference/get-license-status.md" >}}) | the licensing mode and its limits |

## One question, three calls

The pattern behind every scenario — captured from the server, file names shortened:

```text
user              agent                         Search MCP server
 |  "Which contracts mention a termination penalty?"           |
 |---------------->|                                           |
 |                 |-- add_to_index ------------------------->|  reads the
 |                 |   {"documentsPath":"contracts",           |  files once,
 |                 |    "indexName":"contracts"}               |  stores text
 |                 |<- "5 document(s) added, 5 in the index" --|
 |                 |-- search ------------------------------->|
 |                 |   {"query":"termination AND penalty"}     |
 |                 |<- {"documentsFound":3, "documents":[ ... |  which files
 |                 |     "contracts/acme-...-agreement.pdf",   |
 |                 |     "contracts/globex-...-agreement.docx",|
 |                 |     "contracts/initech-nda.docx" ]}       |
 |                 |-- get_text_fragments ------------------->|
 |                 |   {"query":"\"termination penalty\""}     |
 |                 |<- === contracts/acme-...-agreement.pdf ===|  the passages
 |                 |   ...a termination penalty of three       |
 |                 |   monthly fees applies...                 |
 |<- "Two contracts carry one: Acme (3 monthly fees, 12.3),    |
 |    Globex (50% of the remaining fees). Initech excludes it."|
```

Indexing is the one slow step. After it, each question costs a search and a fragments call, however large the folder — and the answer cites the file and the passage, so you can check it.

## Grounded answers without a vector database

You do not need embeddings to ask questions about a folder of documents. An agent with full-text search finds the files, reads only the matching passages, and reformulates when a query misses — research on agentic retrieval has measured keyword tools in an agent loop reaching most of the answer quality of an embedding pipeline ([arXiv 2602.23368](https://arxiv.org/abs/2602.23368v1)). What you give up is matching by meaning: the agent has to try the words the documents use. What you gain is a local index with no model calls, results you can explain, and an index that stays current with one call.

```text
Embedding pipeline                       Agentic full-text search
-------------------------------------    -------------------------------------
split, embed, store in a vector store    add_to_index: a local full-text index
embed the question, take top-k chunks    agent searches, refines, searches again
paste the chunks into the prompt         get_text_fragments: quoted passages
re-embed when documents change           update_index re-reads what changed
needs an embedding model and a store     no model calls, no network at runtime
matches meaning; ranking is opaque       matches words; every hit is explainable
```

## The query language in one table

Quote phrases. Put `content:` in front of a date range. The rest is what you would guess:

| Query | Finds |
|---|---|
| `"termination penalty"` | the exact phrase — **unquoted words separated by spaces match nothing** |
| `termination AND penalty` · `renewal OR renews` | both words / either word, anywhere in the document |
| `terminat?` · `renewl` with `fuzzy: true` | one unknown character / near spellings |
| `content: daterange(2026-07-01 ~~ 2026-09-30)` | dates written in the documents in that range |
| `content: (100000 ~~ 999999)` | whole numbers in that range |
| `filename: q3` · `^inv[0-9]+` | a word in the file name / a regular expression |

Full table, with the traps: [`search`]({{< ref "search/mcp/tools-reference/search.md" >}}#query-language).

## Scenarios

* [Search a folder of contracts]({{< ref "search/mcp/use-cases/search-a-folder-of-contracts.md" >}}) — which contracts say X, and where.
* [Quote matching passages, not whole files]({{< ref "search/mcp/use-cases/quote-matching-passages-not-whole-files.md" >}}) — grounded answers on a small context.
* [Keep an index up to date]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}}) — one index per project, refreshed in one call.
* [Search the documents in a code repository]({{< ref "search/mcp/use-cases/search-project-documents-from-claude-code.md" >}}) — the PDFs and Word files grep cannot read.
* [Search email archives and attachments]({{< ref "search/mcp/use-cases/search-email-archives-locally.md" >}}) — PST, OST, MSG, EML, and what is attached.
* [Find documents by date and amount]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}}) — date ranges, number ranges, file names.
* [On-premise architecture]({{< ref "search/mcp/use-cases/on-premise-document-search.md" >}}) — what stays on the machine, and what travels.

## Install for your platform

Installation, prerequisites, and client configuration are platform-specific; the tools and licensing model are the same everywhere.

| Platform | Status | Install and setup |
|---|---|---|
| .NET | **Available** (Docker only) | [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}) |
| Java | Planned | [Tell us you need it](https://forum.groupdocs.com/c/search/16) |
| Python | Planned | [Tell us you need it](https://forum.groupdocs.com/c/search/16) |
| Node.js | Planned | [Tell us you need it](https://forum.groupdocs.com/c/search/16) |

## Supported AI clients

| Client | How it connects |
|---|---|
| Claude Desktop | `claude_desktop_config.json` |
| Claude Code | `claude mcp add` CLI |
| VS Code / GitHub Copilot | user-level or workspace `mcp.json` |
| Visual Studio 2022 (17.14+) | `.mcp.json` in the solution root |
| Cursor | `~/.cursor/mcp.json` |
| Windsurf | `~/.codeium/windsurf/mcp_config.json` |
| Cline | Cline MCP settings |
| Codex CLI | `codex mcp add` CLI |
| JetBrains Rider | manual registration (Settings → AI Assistant → MCP) |

Exact config blocks for every client: [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}).

## How it works

```text
+--------------+        +----------------------+        +--------------------+
|  AI client   | stdio  |  Search MCP server   | reads  | documents          |
| Claude, VS   |<------>|  Docker container,   |<-------| /data: contracts/, |
| Code, Cursor |  JSON- |  GroupDocs.Search    |        | mail/, reports/    |
|              |  RPC   |  engine in-process   | reads/ +--------------------+
+--------------+        |                      | writes +--------------------+
                        |                      |<------>| index store        |
                        +----------------------+        | /data/.groupdocs-  |
                                                        | search-index/<name>|
  nothing listens on a port; no network at runtime      +--------------------+
```

The server uses MCP's **local stdio transport**: your AI client starts the container as a child process and talks to it over standard input/output. No inbound ports, no external endpoints, no telemetry. Unlike the other GroupDocs MCP servers it keeps **state** — the index — inside the folder you mount, so it survives restarts. What stays local and what reaches your model provider: [On-premise architecture]({{< ref "search/mcp/use-cases/on-premise-document-search.md" >}}).

## Delivery channel

| | Docker (the only channel) |
|---|---|
| Prerequisites | Docker. No .NET SDK — the runtime is in the image |
| Image | `ghcr.io/groupdocs-search/search-net-mcp` · `groupdocs/search-net-mcp` |
| Architectures | linux/amd64 + linux/arm64 (Apple Silicon native) |
| Size | about 380 MiB to download — pull it once, deliberately |

## Resources

* [Quick start]({{< ref "search/mcp/getting-started/_index.md" >}}) · [Use cases]({{< ref "search/mcp/use-cases/_index.md" >}}) · [Supported formats]({{< ref "search/mcp/supported-formats.md" >}}) · [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}})
* GitHub: [server source](https://github.com/groupdocs-search/GroupDocs.Search.Mcp) · [integration tests](https://github.com/groupdocs-search/GroupDocs.Search.Mcp.Tests)
* [Docker image](https://github.com/orgs/groupdocs-search/packages/container/package/search-net-mcp) · [MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.groupdocs-search/groupdocs-search-mcp)
* The library behind it: [GroupDocs.Search for .NET]({{< ref "search/net/_index.md" >}})
* Questions: [Search forum](https://forum.groupdocs.com/c/search/16)
