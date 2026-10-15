---
id: mcp-install-in-ai-clients
url: search/net/mcp/install-in-ai-clients
title: Register in AI clients
weight: 5
description: "Register the GroupDocs.Search MCP server in any AI client — Claude Desktop, Claude Code, VS Code, Visual Studio 2022, Cursor, Windsurf, Cline, or Codex — with the exact Docker-based config for each."
keywords: add MCP server to Claude Desktop config, docker MCP server config, VS Code mcp.json docker, Cursor MCP config file location, claude mcp add docker
productName: GroupDocs.Search MCP Server for .NET
toc: True
---

Register the GroupDocs.Search MCP server in any AI client with the exact config file location and JSON block for each. **Restart the client** after any registration change.

{{< alert style="info" >}}
Every entry below uses **Docker**, because that is the only channel for this product. The packed tool is 280.9 MiB — the GroupDocs.Search engine is about 200 MiB with its ONNX and SkiaSharp natives — which is over nuget.org's 250 MB package limit, so `dnx` and `dotnet tool install` are not available for this product.
{{< /alert >}}

## One-click install

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF)](https://vscode.dev/redirect/mcp/install?name=groupdocs-search&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22--rm%22%2C%22-i%22%2C%22-v%22%2C%22%2Fpath%2Fto%2Fdocuments%3A%2Fdata%22%2C%22-e%22%2C%22GROUPDOCS_MCP_STORAGE_PATH%3D%2Fdata%22%2C%22-e%22%2C%22GROUPDOCS_MCP_OUTPUT_PATH%3D%2Fdata%22%2C%22-e%22%2C%22GROUPDOCS_LICENSE_PATH%3D%22%2C%22ghcr.io%2Fgroupdocs-search%2Fsearch-net-mcp%3Alatest%22%5D%7D) [![Install in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5)](https://insiders.vscode.dev/redirect/mcp/install?name=groupdocs-search&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22--rm%22%2C%22-i%22%2C%22-v%22%2C%22%2Fpath%2Fto%2Fdocuments%3A%2Fdata%22%2C%22-e%22%2C%22GROUPDOCS_MCP_STORAGE_PATH%3D%2Fdata%22%2C%22-e%22%2C%22GROUPDOCS_MCP_OUTPUT_PATH%3D%2Fdata%22%2C%22-e%22%2C%22GROUPDOCS_LICENSE_PATH%3D%22%2C%22ghcr.io%2Fgroupdocs-search%2Fsearch-net-mcp%3Alatest%22%5D%7D&quality=insiders) [![Add to Cursor](https://img.shields.io/badge/Cursor-Add_MCP-111111)](https://cursor.com/en/install-mcp?name=groupdocs-search&config=eyJjb21tYW5kIjoiZG9ja2VyIiwiYXJncyI6WyJydW4iLCItLXJtIiwiLWkiLCItdiIsIi9wYXRoL3RvL2RvY3VtZW50czovZGF0YSIsIi1lIiwiR1JPVVBET0NTX01DUF9TVE9SQUdFX1BBVEg9L2RhdGEiLCItZSIsIkdST1VQRE9DU19NQ1BfT1VUUFVUX1BBVEg9L2RhdGEiLCItZSIsIkdST1VQRE9DU19MSUNFTlNFX1BBVEg9IiwiZ2hjci5pby9ncm91cGRvY3Mtc2VhcmNoL3NlYXJjaC1uZXQtbWNwOmxhdGVzdCJdfQ%3D%3D)

{{< alert style="info" >}}
One-click installs pre-fill every supported setting. After installing, **edit the placeholder documents folder** (`/path/to/documents`) — that host path is what the container sees as `/data`, and where the index is written. **On Linux**, also add `"--user", "1000:1000"` (your `id -u`:`id -g`) — [why]({{< ref "search/net/mcp/linux-installation.md" >}}#run-the-container-as-your-user).
{{< /alert >}}

The standard server entry:

```json
{
  "command": "docker",
  "args": [
    "run", "--rm", "-i",
    "-v", "/path/to/documents:/data",
    "ghcr.io/groupdocs-search/search-net-mcp:latest"
  ]
}
```

Everything after the image name is passed to the server; everything before it configures the container. Environment variables therefore go **before** the image, as `-e` flags — see [Configuration]({{< ref "search/net/mcp/configuration.md" >}}).

## Claude Desktop

Add to `claude_desktop_config.json` under the `mcpServers` key:

* Windows: `%APPDATA%\Claude\claude_desktop_config.json`
* macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
* Linux: `~/.config/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "groupdocs-search": {
      "type": "stdio",
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

## Claude Code

```bash
claude mcp add groupdocs-search -- docker run --rm -i \
  -v /path/to/documents:/data ghcr.io/groupdocs-search/search-net-mcp:latest
```

`--scope user` makes it machine-wide. To search the documents of the repository you are in, run it from the repository root with `-v "$(pwd)":/data` — [search documents in a code repository]({{< ref "search/mcp/use-cases/search-project-documents-from-claude-code.md" >}}).

## VS Code / GitHub Copilot

User-level `mcp.json` (root key `servers`), or workspace `.vscode/mcp.json`:

* Windows: `%APPDATA%\Code\User\mcp.json` · macOS: `~/Library/Application Support/Code/User/mcp.json` · Linux: `~/.config/Code/User/mcp.json`

```json
{
  "servers": {
    "groupdocs-search": {
      "type": "stdio",
      "command": "docker",
      "args": [ "run", "--rm", "-i",
                "-v", "${workspaceFolder}:/data",
                "ghcr.io/groupdocs-search/search-net-mcp:latest" ]
    }
  }
}
```

`${workspaceFolder}` mounts the open folder, so the agent searches the project you have open.

## Visual Studio 2022 (17.14+)

Create `.mcp.json` in your **solution root** — same `servers` shape as VS Code.

## Cursor

`~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project), root key `mcpServers` — same entry as Claude Desktop.

## Windsurf

`~/.codeium/windsurf/mcp_config.json`, root key `mcpServers` (Settings → Cascade → Manage MCP servers → View raw config), then refresh the server list.

## Cline

VS Code globalStorage: `…/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`, root key `mcpServers`.

## Codex CLI

```bash
codex mcp add groupdocs-search -- docker run --rm -i \
  -v /path/to/documents:/data ghcr.io/groupdocs-search/search-net-mcp:latest
```

Or add a `[mcp_servers.groupdocs-search]` table to `~/.codex/config.toml`.

## JetBrains Rider

Register manually: Settings → Tools → AI Assistant → Model Context Protocol (MCP) → Add, using the standard entry above.

## Pinning a version

Replace `:latest` with `:26.10.1` in any entry above. Recommended for shared or committed configurations, so a new release cannot change behaviour under a running workflow.

Ready-made config files for each client live in [`install/generated/`](https://github.com/groupdocs-search/GroupDocs.Search.Mcp/tree/master/install/generated) in the server repository.
