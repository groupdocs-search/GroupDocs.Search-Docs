---
id: mcp-net
url: search/net/mcp
title: MCP server for .NET
linkTitle: MCP Server
weight: 7
description: "Install and configure the GroupDocs.Search MCP server for .NET — a Docker-only product: per-OS Docker setup, one-click client links, the index location, and the environment-variable reference."
keywords: GroupDocs.Search MCP .NET, MCP server Docker image, search MCP install, Model Context Protocol .NET, document search server docker
productName: GroupDocs.Search MCP Server for .NET
hideChildren: True
toc: True
---

Everything needed to **install and run** the GroupDocs.Search MCP server on the .NET platform. What the server *does* — its tools, use cases, and licensing model — is platform-independent and lives in the [MCP server section]({{< ref "search/mcp/_index.md" >}}).

{{< alert style="info" >}}
**This product ships via Docker only.** The packed tool is 280.9 MiB — the GroupDocs.Search engine is about 200 MiB with its ONNX and SkiaSharp natives — which is over nuget.org's 250 MB package limit, so `dnx` and `dotnet tool install` are not available for this product.
{{< /alert >}}

| The .NET build at a glance | |
|---|---|
| Container images | `ghcr.io/groupdocs-search/search-net-mcp` · `groupdocs/search-net-mcp` |
| One-command run | `docker run --rm -i -v $(pwd)/documents:/data ghcr.io/groupdocs-search/search-net-mcp:latest` |
| Current version | **26.10.1** (tagged `:latest` and `:26.10.1`) |
| Prerequisites | Docker. No .NET SDK — the runtime is inside the image |
| Architectures | linux/amd64 + linux/arm64 (Apple Silicon native) |
| State | indexes in `/data/.groupdocs-search-index`, or `GROUPDOCS_MCP_INDEX_PATH` |
| Source | [GroupDocs.Search.Mcp on GitHub](https://github.com/groupdocs-search/GroupDocs.Search.Mcp) |
| Release notes | [changelog](https://github.com/groupdocs-search/GroupDocs.Search.Mcp/tree/master/changelog) · [GitHub releases](https://github.com/groupdocs-search/GroupDocs.Search.Mcp/releases) |

## Start here

1. **Install Docker** for your operating system — [Windows]({{< ref "search/net/mcp/windows-installation.md" >}}) · [Linux]({{< ref "search/net/mcp/linux-installation.md" >}}) · [macOS]({{< ref "search/net/mcp/macos-installation.md" >}})
2. **Register it in your AI client** — [one-click links and per-client configs]({{< ref "search/net/mcp/install-in-ai-clients.md" >}})
3. **Point it at your documents** — [configuration]({{< ref "search/net/mcp/configuration.md" >}})
4. **License it** — evaluation, a license file, or metered keys: [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}})

## In this section

* [Install on Windows]({{< ref "search/net/mcp/windows-installation.md" >}})
* [Install on Linux]({{< ref "search/net/mcp/linux-installation.md" >}}) — including running the container as your user
* [Install on macOS]({{< ref "search/net/mcp/macos-installation.md" >}})
* [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}) — VS Code, Cursor, Claude, Visual Studio, Windsurf, Cline, Codex, Rider
* [Configuration]({{< ref "search/net/mcp/configuration.md" >}}) — volumes, index location, license, metered keys
* [System requirements]({{< ref "search/net/mcp/system-requirements.md" >}})
* [Troubleshooting (.NET)]({{< ref "search/net/mcp/troubleshooting.md" >}}) — Docker daemon, volumes, file ownership, image pulls

## Platform-independent reference

* [Tools reference]({{< ref "search/mcp/tools-reference/_index.md" >}}) — `add_to_index`, `search`, `get_text_fragments`, `update_index`, `remove_from_index`, `get_index_info`, `create_index`, `get_license_status`
* [Use cases]({{< ref "search/mcp/use-cases/_index.md" >}}) · [Supported formats]({{< ref "search/mcp/supported-formats.md" >}}) · [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}})
