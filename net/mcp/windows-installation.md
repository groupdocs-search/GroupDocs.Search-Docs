---
id: mcp-windows-installation
url: search/net/mcp/windows-installation
title: Install on Windows
weight: 2
description: "Install the GroupDocs.Search MCP server on Windows with Docker Desktop and register it in Claude Desktop, VS Code, Cursor and other clients."
keywords: install MCP server Windows, MCP server Docker Desktop Windows, Claude Desktop MCP Windows config location, search documents MCP Windows
productName: GroupDocs.Search MCP Server for .NET
toc: True
---

Install the GroupDocs.Search MCP server on **Windows** with Docker Desktop:

```powershell
winget install --id Docker.DockerDesktop
docker pull ghcr.io/groupdocs-search/search-net-mcp:latest
docker run --rm -i -v C:/Docs:/data `
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

Then register it in your AI client — [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}) has the exact block per client — and restart the client.

{{< alert style="info" >}}
**Docker is the only channel for this product.** The packed tool is 280.9 MiB — the GroupDocs.Search engine is about 200 MiB with its ONNX and SkiaSharp natives — which is over nuget.org's 250 MB package limit, so `dnx` and `dotnet tool install` are not available for this product.
{{< /alert >}}

## Prerequisites

Docker Desktop. **Start it once** after installing so it finishes its WSL2 first-run setup. No .NET SDK is required: the runtime and every native dependency live inside the image (about 380 MiB to download — pull it before your client first needs it).

## Where the index goes

The index is written to `C:/Docs/.groupdocs-search-index` — inside the folder you mount — so it survives `--rm` and the next container finds it. Keep documents read-only and the index elsewhere: [Configuration]({{< ref "search/net/mcp/configuration.md" >}}#read-only-documents-and-a-separate-index).

Client config locations on Windows: Claude Desktop `%APPDATA%\Claude\claude_desktop_config.json`, VS Code user-level `%APPDATA%\Code\User\mcp.json` — full list with JSON blocks in [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}).

## Verify

Ask your agent *"list your GroupDocs search tools and the license status"* — it should name eight tools. A scripted check: [verifying an installation]({{< ref "search/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end).

## Windows-specific troubleshooting

* **"docker daemon not reachable"** — start Docker Desktop.
* **Drive sharing** — Docker Desktop must be allowed to mount the drive your documents live on (Settings → Resources → File sharing), or the container starts with an empty `/data`.
* **Path form** — use forward slashes in the volume argument (`C:/Docs:/data`); backslashes confuse the Docker CLI on Windows.

Installing on a different OS? [Linux]({{< ref "search/net/mcp/linux-installation.md" >}}) · [macOS]({{< ref "search/net/mcp/macos-installation.md" >}})
