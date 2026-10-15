---
id: mcp-macos-installation
url: search/net/mcp/macos-installation
title: Install on macOS
weight: 4
description: "Install the GroupDocs.Search MCP server on macOS (Intel and Apple Silicon) with Docker Desktop — multi-arch images run natively on M-series Macs."
keywords: install MCP server macOS, MCP server Mac Apple Silicon docker, Claude Desktop MCP config Mac location, search documents MCP Mac
productName: GroupDocs.Search MCP Server for .NET
toc: True
---

Install the GroupDocs.Search MCP server on **macOS** — Intel and Apple Silicon — with Docker Desktop:

```bash
brew install --cask docker
docker pull ghcr.io/groupdocs-search/search-net-mcp:latest
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

Then register it in your AI client — [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}) has the exact block per client — and restart the client.

{{< alert style="info" >}}
**Docker is the only channel for this product.** The packed tool is 280.9 MiB — the GroupDocs.Search engine is about 200 MiB with its ONNX and SkiaSharp natives — which is over nuget.org's 250 MB package limit, so `dnx` and `dotnet tool install` are not available for this product.
{{< /alert >}}

## Prerequisites

* **Docker Desktop** — launch it once from /Applications to finish first-run setup.
* **Apple Silicon:** the images are multi-arch (`linux/arm64` + `linux/amd64`), so M-series Macs run them **natively**.

Client config locations on macOS: Claude Desktop `~/Library/Application Support/Claude/claude_desktop_config.json`, VS Code user-level `~/Library/Application Support/Code/User/mcp.json`, Cursor `~/.cursor/mcp.json` — full list in [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}).

## Verify

Ask your agent *"list your GroupDocs search tools and the license status"*. A scripted check: [verifying an installation]({{< ref "search/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end).

## macOS-specific troubleshooting

* **Docker daemon not reachable** — Docker Desktop must be running (menu-bar whale icon).
* **File sharing** — Docker Desktop only mounts paths it is allowed to share (Settings → Resources → File sharing); a folder outside those paths mounts empty.
* **First pull is slow** — about 380 MiB; `docker pull` it once before your client first needs it.

Installing on a different OS? [Windows]({{< ref "search/net/mcp/windows-installation.md" >}}) · [Linux]({{< ref "search/net/mcp/linux-installation.md" >}})
