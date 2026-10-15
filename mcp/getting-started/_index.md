---
id: mcp-getting-started
url: search/mcp/getting-started
title: Quick start
weight: 1
description: "Run the GroupDocs.Search MCP server with Docker, register it in Claude Desktop, Claude Code, VS Code or Cursor, and ask your first question about a folder of documents — in five minutes."
keywords: how to install MCP server, search documents MCP quick start, add MCP server to Claude Desktop, docker MCP server setup
productName: GroupDocs.Search MCP Server
hideChildren: True
toc: True
---

{{< alert style="info" >}}
These steps run the **.NET** build of the server, which ships as a Docker image — the only platform available today. Platform-independent material (tools, use cases, licensing) lives here and applies to every future platform.
{{< /alert >}}

Run the GroupDocs.Search MCP server with Docker, register it in your AI client, and ask your first question about a folder of documents — locally, in five minutes.

## 1. Pull the image

```bash
docker pull ghcr.io/groupdocs-search/search-net-mcp:latest
```

About 380 MiB. Pulling it once now means your client does not time out waiting for it on the first call.

## 2. Register it in your AI client

**Claude Code:**

```bash
claude mcp add groupdocs-search -- docker run --rm -i \
  -v /path/to/documents:/data ghcr.io/groupdocs-search/search-net-mcp:latest
```

**Claude Desktop, Cursor, Windsurf** — add under `mcpServers` in the client's config file:

```json
{
  "mcpServers": {
    "groupdocs-search": {
      "command": "docker",
      "args": [
        "run", "--rm", "-i",
        "-v", "/path/to/documents:/data",
        "ghcr.io/groupdocs-search/search-net-mcp:latest"
      ]
    }
  }
}
```

Replace `/path/to/documents` with the folder the agent may search. **On Linux**, add `"--user", "1000:1000"` (your numbers from `id -u` and `id -g`) after `"-i"` — [why]({{< ref "search/net/mcp/linux-installation.md" >}}#run-the-container-as-your-user). VS Code, Visual Studio, Cline, Codex and one-click install links: [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}).

Then **restart your AI client** so it picks up the new server.

## 3. Your first question

Put a folder of documents (say `contracts/`) into the documents folder, then ask:

> Index the contracts folder, then tell me which contracts mention a termination penalty and quote the clause

The agent calls `add_to_index`, then `search`, then `get_text_fragments`, and answers with the file names and the quoted passages. The index stays on disk, so the next question does not re-read anything.

The index is written to `.groupdocs-search-index` inside your documents folder; the server never indexes that folder itself. In a code repository, add it to `.gitignore`.

## Where to go next

* **The pattern in detail:** [Search documents with AI agents]({{< ref "search/mcp/use-cases/search-documents-with-ai-agents.md" >}})
* **Your OS:** [Windows]({{< ref "search/net/mcp/windows-installation.md" >}}) · [Linux]({{< ref "search/net/mcp/linux-installation.md" >}}) · [macOS]({{< ref "search/net/mcp/macos-installation.md" >}})
* **Read-only documents, a separate index volume, metered keys:** [Configuration]({{< ref "search/net/mcp/configuration.md" >}})
* **Licensing:** evaluation mode caps an index at 100 documents and a result at 10 — [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}).
