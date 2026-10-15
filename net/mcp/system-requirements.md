---
id: mcp-system-requirements
url: search/net/mcp/system-requirements
title: System requirements
weight: 8
description: "The GroupDocs.Search MCP server runs anywhere Docker runs — image size, architectures, disk for indexes, and what the host needs."
keywords: MCP server requirements, does MCP server need Docker, search MCP docker requirements, index disk space
productName: GroupDocs.Search MCP Server for .NET
toc: True
---

The GroupDocs.Search MCP server runs **anywhere Docker runs**. There is no NuGet channel for this product — The packed tool is 280.9 MiB — the GroupDocs.Search engine is about 200 MiB with its ONNX and SkiaSharp natives — which is over nuget.org's 250 MB package limit, so `dnx` and `dotnet tool install` are not available for this product.

| | Docker channel (the only channel) |
|---|---|
| Runtime | Docker Desktop / Docker engine |
| .NET SDK | **not required** — the runtime is inside the image |
| Native dependencies | bundled in the image |
| Architectures | linux/amd64 + linux/arm64 (Apple Silicon native) |
| Image | about 380 MiB to download (382 MiB amd64, 378 MiB arm64 for 26.10.1) |
| Disk for indexes | grows with the extracted text of what you index; plan on a fraction of the documents' size, more for text-heavy corpora |
| Network | pull from ghcr.io / docker.io at install only — none at runtime (metered licensing reports usage) |
| Linux | the container user must be able to write the index folder — `--user "$(id -u):$(id -g)"` |

Pull the image deliberately (`docker pull ghcr.io/groupdocs-search/search-net-mcp:latest`) rather than letting a client hit it on the first tool call and time out.

Per-OS setup: [Windows]({{< ref "search/net/mcp/windows-installation.md" >}}) · [Linux]({{< ref "search/net/mcp/linux-installation.md" >}}) · [macOS]({{< ref "search/net/mcp/macos-installation.md" >}})

AI client requirements: any MCP-compatible client (Claude Desktop, Claude Code, VS Code / GitHub Copilot, Visual Studio 2022 17.14+, Cursor, Windsurf, Cline, Codex CLI) — see [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}).

The engine's own platform requirements are those of the library: [GroupDocs.Search system requirements]({{< ref "search/net/getting-started/system-requirements.md" >}}).
