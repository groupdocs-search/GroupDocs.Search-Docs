---
id: mcp-configuration
url: search/net/mcp/configuration
title: Configuration
weight: 6
description: "Configure the GroupDocs.Search MCP server: the documents volume, where indexes are stored, read-only documents with a separate index volume, license path, and metered keys."
keywords: MCP server environment variables, docker volume MCP server, GROUPDOCS_MCP_INDEX_PATH, read-only documents index volume, pin MCP server version
productName: GroupDocs.Search MCP Server for .NET
toc: True
---

Configure the GroupDocs.Search MCP server on the `docker run` command line — volumes and `-e` variables **before the image name**:

```bash
docker run --rm -i \
  -v /path/to/documents:/data \
  -v /path/to/license-folder:/license:ro \
  -e GROUPDOCS_LICENSE_PATH=/license/GroupDocs.Search.lic \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

## Environment variables

| Variable | Description | Default |
|---|---|---|
| `GROUPDOCS_MCP_STORAGE_PATH` | Documents folder **inside the container**; tool paths are relative to it | `/data` in the image |
| `GROUPDOCS_MCP_INDEX_PATH` | Where indexes are stored; each index is a sub-folder named after it | `<output or storage>/.groupdocs-search-index` |
| `GROUPDOCS_MCP_OUTPUT_PATH` | Optional writable folder; indexes go under it when `GROUPDOCS_MCP_INDEX_PATH` is not set | same as storage |
| `GROUPDOCS_LICENSE_PATH` | Path **inside the container** to a GroupDocs license file | empty = evaluation mode |
| `GROUPDOCS_METERED_PUBLIC_KEY` | Metered public key. Requires the private key too; takes precedence over the license file | unset |
| `GROUPDOCS_METERED_PRIVATE_KEY` | Metered private key. **Treat as a secret** — see [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}) | unset |

These are **container paths**, not host paths: `/data` is whatever host folder you mounted there.

## Where indexes live

By default, in `.groupdocs-search-index` inside the documents folder — `/data/.groupdocs-search-index/contracts` for an index named `contracts`. That keeps one mount for everything: the index survives `--rm`, and the next container finds it. The server never indexes its own index folder. In a code repository, add `.groupdocs-search-index/` to `.gitignore`.

**The index contains the documents' text.** Protect and dispose of it like the documents.

## Read-only documents and a separate index

Mount the documents read-only and give the index its own writable volume — **always both together**:

```bash
docker run --rm -i \
  -v /path/to/documents:/data:ro \
  -v /path/to/index:/index \
  -e GROUPDOCS_MCP_INDEX_PATH=/index \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

Without the `/index` volume — or without `GROUPDOCS_MCP_INDEX_PATH` — the server tries to write the index into the read-only `/data` and indexing fails with *"The index folder … is not writable by the server process"*.

## Linux: run as your user

The image runs as a non-root user (uid 1654). On Linux a mounted folder keeps its host owner, so a folder you own is **read-only inside the container** and the index cannot be written — indexing fails with *"The index folder … is not writable by the server process"*. Run the container as your user. Docker Desktop on Windows and macOS does not enforce this. Add `--user "$(id -u):$(id -g)"` — or the numbers, in a JSON config: [Install on Linux]({{< ref "search/net/mcp/linux-installation.md" >}}#run-the-container-as-your-user).

## Metered keys

Forward the keys **by name** so they never appear in a config file:

```bash
docker run --rm -i -v /path/to/documents:/data \
  -e GROUPDOCS_METERED_PUBLIC_KEY -e GROUPDOCS_METERED_PRIVATE_KEY \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

## Indexes from older versions

Indexes built by GroupDocs.Search 22.7–26.8 are converted to the 26.10 format on first use. The original is kept as `<index>.pre-26.10-backup` next to it, and the tool response says a conversion happened. Delete the backup once you are satisfied.

## Docker Compose

```yaml
services:
  groupdocs-search:
    image: ghcr.io/groupdocs-search/search-net-mcp:26.10.1
    stdin_open: true
    volumes:
      - ./documents:/data
```

A Compose service suits supervised or shared hosts; most desktop clients start the container themselves per session.
