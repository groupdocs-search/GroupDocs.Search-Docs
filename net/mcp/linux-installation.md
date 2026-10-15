---
id: mcp-linux-installation
url: search/net/mcp/linux-installation
title: Install on Linux
weight: 3
description: "Install the GroupDocs.Search MCP server on Linux with the Docker engine — run the container as your user so it can write its index, and register it in your AI client."
keywords: install MCP server Linux, MCP server docker Ubuntu, docker --user MCP server, index not writable docker, self-hosted document search Linux
productName: GroupDocs.Search MCP Server for .NET
toc: True
---

Install the GroupDocs.Search MCP server on **Linux** with the Docker engine, and run the container **as your user** so it can write its index:

```bash
sudo apt-get install -y docker.io
docker pull ghcr.io/groupdocs-search/search-net-mcp:latest
docker run --rm -i --user "$(id -u):$(id -g)" \
  -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

Then register it in your AI client — [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}) has the exact block per client — and restart the client.

{{< alert style="info" >}}
**Docker is the only channel for this product.** The packed tool is 280.9 MiB — the GroupDocs.Search engine is about 200 MiB with its ONNX and SkiaSharp natives — which is over nuget.org's 250 MB package limit, so `dnx` and `dotnet tool install` are not available for this product. That also means there are no native libraries to install — everything is inside the image.
{{< /alert >}}

## Prerequisites

* **Docker engine.** Add yourself to the `docker` group (`sudo usermod -aG docker $USER`) and **log out and in** (or `newgrp docker`) before the first run.
* About 380 MiB of disk for the image.

## Run the container as your user

The image runs as a non-root user (uid 1654). On Linux a mounted folder keeps its host owner, so a folder you own is **read-only inside the container** and the index cannot be written — indexing fails with *"The index folder … is not writable by the server process"*. Run the container as your user. Docker Desktop on Windows and macOS does not enforce this.

```bash
docker run --rm -i --user "$(id -u):$(id -g)" \
  -v /path/to/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

MCP client configs are JSON and run no shell, so `$(id -u)` is not expanded there. Put the numbers in directly — `id -u` and `id -g` print them, typically `1000`:

```json
"args": [ "run", "--rm", "-i", "--user", "1000:1000",
          "-v", "/path/to/documents:/data",
          "ghcr.io/groupdocs-search/search-net-mcp:latest" ]
```

The alternative is to keep documents read-only and give the index its own writable volume — [Configuration]({{< ref "search/net/mcp/configuration.md" >}}#read-only-documents-and-a-separate-index).

Client config locations on Linux: Claude Desktop `~/.config/Claude/claude_desktop_config.json`, VS Code user-level `~/.config/Code/User/mcp.json` — full list in [Register in AI clients]({{< ref "search/net/mcp/install-in-ai-clients.md" >}}).

## Verify

Ask your agent *"list your GroupDocs search tools and the license status"*. A scripted check: [verifying an installation]({{< ref "search/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end).

## Linux-specific troubleshooting

* **`docker: permission denied`** — you are not in the `docker` group yet; `newgrp docker` or re-login.
* **"The index folder … is not writable"** — the container is not running as you; add `--user` as above.
* **Empty `/data` in the container** — the host path did not exist, so Docker created an empty directory. Create the folder first.
* **SELinux hosts** — add `:z` to the volume (`-v $(pwd)/documents:/data:z`) if the container cannot read mounted files.

Installing on a different OS? [Windows]({{< ref "search/net/mcp/windows-installation.md" >}}) · [macOS]({{< ref "search/net/mcp/macos-installation.md" >}})
