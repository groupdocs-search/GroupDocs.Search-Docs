---
id: mcp-troubleshooting-faq
url: search/mcp/troubleshooting-faq
title: Troubleshooting & FAQ
weight: 5
description: "Solutions to the most common GroupDocs.Search MCP server issues — server not appearing in the client, startup failures, missing native dependencies, and first-launch timeouts."
keywords: MCP server not showing up in Claude Desktop, Claude can't see MCP tools, MCP server failed to start, docker daemon not reachable, search returns nothing, unquoted phrase search, index not writable docker linux, search documents with AI agent
productName: GroupDocs.Search MCP Server
toc: True
---

Solutions to the most common GroupDocs.Search MCP server issues — server not appearing in the client, startup failures, missing native dependencies, and first-launch timeouts.

{{< alert style="info" >}}
**Platform-specific troubleshooting:** this product runs as a Docker container, so runtime problems are Docker problems — the daemon, volume mounts, file ownership on Linux, and the first image pull. See [Troubleshooting (.NET)]({{< ref "search/net/mcp/troubleshooting.md" >}}). The issues on this page apply to every platform.
{{< /alert >}}

## Why is my MCP server not showing up in Claude Desktop?

1. **Restart the client** — every client reads its MCP config only at startup.
2. Check the config file location for your OS ([per-client reference]({{< ref "search/net/mcp/install-in-ai-clients.md" >}})) and that the entry sits under the right root key (`mcpServers` for Claude Desktop/Cursor/Windsurf, `servers` for VS Code/VS 2022).
3. Validate the JSON — a trailing comma silently breaks the whole file. If you used the [installer](https://github.com/groupdocs/GroupDocs.Mcp.Installer), a timestamped `.bak` of your previous config sits next to the file for comparison.

## The first tool call is slow or fails once, then works

A **cold cache**: on the very first use the server's package or image is still downloading while the client is already waiting on the connection. Warming it once fixes it for good — the exact command depends on your build: [.NET]({{< ref "search/net/mcp/troubleshooting.md" >}}#the-first-tool-call-is-slow-or-times-out-once-and-then-works).

## The server fails to start, or a runtime dependency is missing

These are properties of the build you run rather than of MCP, so the fixes live with the platform:

| Symptom | Where the fix is |
|---|---|
| "docker daemon not reachable" | [.NET troubleshooting]({{< ref "search/net/mcp/troubleshooting.md" >}}#docker-daemon-not-reachable) — start Docker Desktop or `dockerd` |
| *"The index folder … is not writable"* (Linux) | [.NET troubleshooting]({{< ref "search/net/mcp/troubleshooting.md" >}}#the-index-folder-is-not-writable-linux) — run the container with `--user` |
| The index is empty after a restart | [.NET troubleshooting]({{< ref "search/net/mcp/troubleshooting.md" >}}#the-index-is-empty-after-a-restart) — `/data` is not a mounted volume |
| You were told to run `dnx` | There is no NuGet package for this product — [why]({{< ref "search/net/mcp/troubleshooting.md" >}}#why-is-there-no-dnx-command-for-this-product) |

## The agent says a file does not exist

Pass a path **relative to the storage folder** — `contracts` or `contracts/lease.pdf`, not `/home/me/contracts`. When a path is not found the tool answers with the folder's contents, so the agent can correct itself. In Docker the storage folder is whatever you mounted at `/data` ([configuration]({{< ref "search/net/mcp/configuration.md" >}})).

## Why does a two-word search find nothing?

Because unquoted words separated by spaces do not match in GroupDocs.Search. Quote the phrase — `"termination penalty"` — or join the words with `AND`. The server spots this and adds a `hint` to the empty result, so an agent usually corrects itself on the next call:

```text
"hint": "Unquoted words separated by spaces do not match. Search the phrase
         as \"termination penalty\", or combine the words with AND / OR,
         e.g. termination AND penalty."
```

## Why does a date-range search match files that do not mention the date?

The engine indexes each file's **creation and modification dates** as fields, and a bare `daterange(…)` searches them too. Write `content: daterange(2026-07-01 ~~ 2026-09-30)` to search dates written in the documents — [find documents by date and amount]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}}).

## Why can't I remove one file from the index?

The engine removes **paths that were added**, not files inside an added folder. If you indexed `contracts`, removing `contracts/old.pdf` fails with an explanation; remove `contracts` and add back what you want, or index sub-folders separately when you expect to remove them — [keep an index current]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}}).

## Where is my index, and does it survive a restart?

In `/data/.groupdocs-search-index/<name>` — inside the mounted documents folder — so it survives `--rm` and the next container finds it. With read-only documents, mount a writable folder at `/index` and set `GROUPDOCS_MCP_INDEX_PATH=/index` ([configuration]({{< ref "search/net/mcp/configuration.md" >}})). If you are not mounting `/data` at all, the index lives inside the container and disappears with it.

## Is this semantic or vector search?

No. It is **full-text search**: words, phrases, boolean operators, fuzzy matching, `?` wildcards, regular expressions, date and number ranges. It does not match meaning — the agent compensates by trying synonyms and variants (`renewal OR renews`, `fuzzy: true`), which works well for document questions that use the words the documents use, and poorly for purely conceptual ones. There are no embeddings, no model calls, and no vector database involved.

## Does it OCR scanned PDFs?

No. A scan without a text layer is indexed by its file properties only; its words are pixels. Run OCR first if you need scans in the index.

## Why is there no `dnx` command for Search?

Because there is no NuGet package. The packed tool is 280.9 MiB — the engine alone is about 200 MiB with its ONNX and SkiaSharp natives — which is over nuget.org's 250 MB limit, so this product ships **via Docker only**. See [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}).

## Search or Parser — which one do I need?

[GroupDocs.Parser](/parser/mcp/) reads **one** document you name. Search answers **which** documents, across a folder, and quotes the passages. Pipelines often use both: Search to find the three files that matter, Parser to pull a table out of one of them.

## Verifying an installation end-to-end

Ask your agent *"list your GroupDocs search tools and the license status"* — it should name `add_to_index`, `search`, `get_text_fragments`, `update_index`, `remove_from_index`, `get_index_info`, `create_index`, `get_license_status`. For a scripted check that performs the real MCP handshake and a live call through the engine, see [verifying a .NET installation]({{< ref "search/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end).

## Still stuck?

Post your config (redact license paths) and the client name in the [Search forum](https://forum.groupdocs.com/c/search/16) — we answer MCP questions daily. Bugs: [GitHub issues](https://github.com/groupdocs-search/GroupDocs.Search.Mcp/issues).
