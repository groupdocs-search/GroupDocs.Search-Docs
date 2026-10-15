---
id: mcp-net-troubleshooting
url: search/net/mcp/troubleshooting
title: Troubleshooting (.NET)
linkTitle: Troubleshooting
weight: 9
description: "Fixes for problems specific to the Docker build of the GroupDocs.Search MCP server — index not writable on Linux, index lost after restart, read-only mounts, daemon, slow first pull."
keywords: docker daemon not reachable, index folder not writable docker linux, MCP server read-only file system, docker volume not mounted MCP, MCP server first launch timeout
productName: GroupDocs.Search MCP Server for .NET
toc: True
---

Problems specific to running this server — which means **Docker problems**, since Docker is its only channel. For issues that are the same on every platform (the server not appearing in your client, query syntax, evaluation limits), see [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}}).

## Why is there no `dnx` command for this product?

The packed tool is 280.9 MiB — the GroupDocs.Search engine is about 200 MiB with its ONNX and SkiaSharp natives — which is over nuget.org's 250 MB package limit, so `dnx` and `dotnet tool install` are not available for this product. If you have seen `dnx GroupDocs.<Product>.Mcp --yes` in another GroupDocs MCP guide, that is correct for those products and does not apply here.

## The index folder is not writable (Linux)

The response reads:

```text
Indexing failed for 'contracts': The index folder
'/data/.groupdocs-search-index' is not writable by the server process.
In Docker on Linux the container runs as a non-root user, ...
```

The image runs as a non-root user (uid 1654). On Linux a mounted folder keeps its host owner, so a folder you own is **read-only inside the container** and the index cannot be written — indexing fails with *"The index folder … is not writable by the server process"*. Run the container as your user. Docker Desktop on Windows and macOS does not enforce this. Fix: `--user "$(id -u):$(id -g)"` on the command line, or `"--user", "1000:1000"` in a JSON config — [Install on Linux]({{< ref "search/net/mcp/linux-installation.md" >}}#run-the-container-as-your-user). Or keep documents read-only and mount a writable `/index` volume with `GROUPDOCS_MCP_INDEX_PATH=/index` — [Configuration]({{< ref "search/net/mcp/configuration.md" >}}#read-only-documents-and-a-separate-index).

## Indexing fails with the documents mounted read-only

With `-v …:/data:ro` the default index location inside `/data` cannot be written, and indexing fails with the same *"is not writable"* message. Add a writable index volume: `-v /path/to/index:/index -e GROUPDOCS_MCP_INDEX_PATH=/index`.

## The index is empty after a restart

`/data` was not a mounted host folder, so the index lived inside the container and went away with `--rm`. Mount the documents folder: `-v /path/to/documents:/data`. With a separate index volume, check that `/index` is mounted too.

## "docker daemon not reachable"

Start Docker Desktop (or `dockerd`).

## The agent says the storage folder is empty

The volume did not mount what you expected:

* **The host path does not exist** — Docker silently creates an empty directory instead of failing.
* **File sharing is not enabled** for that location (Docker Desktop → Settings → Resources → File sharing) on Windows and macOS.
* **Windows path form** — use forward slashes: `-v C:/Docs:/data`.

List the folder from inside the container:

```bash
docker run --rm --entrypoint ls \
  -v /path/to/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest -la /data
```

## Paths are relative to the storage folder

Tools take `contracts` or `contracts/lease.pdf`, not `/home/me/contracts`. An unknown path answers with the folder's contents, so the agent can correct itself. In the environment, `GROUPDOCS_MCP_STORAGE_PATH` and `GROUPDOCS_MCP_INDEX_PATH` are **container** paths.

## The first tool call is slow, or times out once and then works

The image is about 380 MiB and on first use your client waits while Docker pulls it. Warm it once:

```bash
docker pull ghcr.io/groupdocs-search/search-net-mcp:latest
```

The first indexing of a large folder also takes time; later questions read the index.

## Pinning a version

`:latest` resolves to the newest release whenever the container starts. For shared or committed configurations, pin the tag — `ghcr.io/groupdocs-search/search-net-mcp:26.10.1`.

## Verifying an installation end-to-end

In your AI client, ask *"list your GroupDocs search tools and the license status"* — the agent should name eight tools and report the licensing mode.

For a scripted check outside any client, the [integration-test repository](https://github.com/groupdocs-search/GroupDocs.Search.Mcp.Tests) has a smoke test that performs the MCP handshake, lists the tools, indexes sample documents and runs a search:

```bash
git clone https://github.com/groupdocs-search/GroupDocs.Search.Mcp.Tests.git
cd GroupDocs.Search.Mcp.Tests/docker-scripts
./04_run-server-with-samples.sh
```

It tests `ghcr.io/groupdocs-search/search-net-mcp:latest` by default (`--image` picks another tag) and ends with `OK` when the image works.

## Still stuck?

Post your `docker run` arguments (redact license paths) and the client name in the [Search forum](https://forum.groupdocs.com/c/search/16). Bugs: [GitHub issues](https://github.com/groupdocs-search/GroupDocs.Search.Mcp/issues).
