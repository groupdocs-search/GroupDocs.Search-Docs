---
id: mcp-uc-on-premise-document-search
url: search/mcp/use-cases/on-premise-document-search
title: "Running GroupDocs MCP servers on-premise: architecture and security model"
linkTitle: On-premise deployment
weight: 8
description: "Run document search for AI agents fully on-premise with the Docker-only GroupDocs.Search server: local stdio transport, a local index, no external endpoints, no telemetry."
keywords: on-premise MCP server, air-gapped document search, MCP security model, local document index no upload, self-hosted search AI agent
productName: GroupDocs.Search MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "Running GroupDocs MCP servers on-premise: architecture and security model"
        description: "Run document search for AI agents fully on-premise with the Docker-only GroupDocs.Search server: local stdio transport, a local index, no external endpoints, no telemetry."
        steps:
        - name: "Run the pinned image inside the perimeter"
          text: "Start the GroupDocs.Search MCP server from its versioned Docker image as a child process of the AI client."
        - name: "Mount only the folders the agent may reach"
          text: "Map the document folder read-write and the license folder read-only."
        - name: "Choose the license mode"
          text: "Use a license file for fully offline operation; metered licensing needs outbound egress for usage reports."
---

Run document search for AI agents **fully on-premise**: the GroupDocs.Search MCP server uses local stdio transport with **no external endpoints, no inbound ports, and no telemetry**, and keeps its index on your disk. This page is the one to send your security reviewer.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## The architecture in one picture

```text
+--------------+        +----------------------+        +--------------------+
|  AI client   | stdio  |  Search MCP server   | reads  | documents          |
| Claude, VS   |<------>|  Docker container,   |<-------| /data: contracts/, |
| Code, Cursor |  JSON- |  GroupDocs.Search    |        | mail/, reports/    |
|              |  RPC   |  engine in-process   | reads/ +--------------------+
+--------------+        |                      | writes +--------------------+
                        |                      |<------>| index store        |
                        +----------------------+        | /data/.groupdocs-  |
                                                        | search-index/<name>|
  nothing listens on a port; no network at runtime      +--------------------+
```

* **Transport:** stdio to a child process — your AI client runs `docker run --rm -i …`. The server never listens on a network socket.
* **Data path:** agent → local container → mounted folders. Documents are read from the volume you mounted; nothing is transmitted anywhere.
* **State:** the index — extracted text and term data — is written to `/data/.groupdocs-search-index` or to the folder in `GROUPDOCS_MCP_INDEX_PATH`. **Treat it with the same classification as the documents**: it contains their text.
* **Network use:** only to pull the image (ghcr.io / docker.io). At runtime the container makes no outbound calls, unless metered licensing is enabled.
* **Telemetry:** none.

## A tighter deployment

```bash
docker run --rm -i \
  -v /srv/documents:/data:ro \
  -v /srv/search-index:/index \
  -v /srv/licenses:/license:ro \
  -e GROUPDOCS_MCP_INDEX_PATH=/index \
  -e GROUPDOCS_LICENSE_PATH=/license/GroupDocs.Search.lic \
  ghcr.io/groupdocs-search/search-net-mcp:26.10.1
```

* **Documents read-only**, the index on its own volume — the container can write nowhere else.
* **Pin the tag** (`:26.10.1`, not `:latest`) so a release cannot change behaviour under a running workflow.
* **Mount deliberately:** the container sees the folders you give it and nothing else — a tighter boundary than a process running as your user.
* On Linux, run with `--user` matching the owner of the index volume.

## The part that still travels

Search produces **document content as text** — file names, matching passages — and that text goes into the conversation. With a cloud-hosted model, *"the Acme agreement charges a termination penalty of three monthly fees"* has reached your model provider: the document did not move, but the passage did.

For sensitive corpora, that is the argument for a locally-hosted model. Otherwise it is worth knowing, and worth phrasing prompts accordingly: *"tell me which files mention the penalty, not the clause"* is a request an agent will honour — `search` returns file names and counts without any document text.

## License management

* **License file** — read from local disk by the local container. Fully offline; the right answer for air-gapped deployments.
* **Metered (pay-per-use)** — reports *usage* to GroupDocs servers, so it needs outbound egress. Document content is never part of that report.

Both: [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}).

## What this fits — honestly

**A good fit:** contract, policy and correspondence search inside a network that does not allow uploads; e-discovery-style questions over local archives; documentation search for engineering teams.

**Not what this is:** a semantic search service or an OCR engine. It matches words, opens 80+ formats including email archives, and quotes passages; matching by meaning and reading scans are steps you add.

## FAQ

**Does any document content leave the machine?** Not from the server. Passages the agent quotes travel in the conversation to your model provider.

**Does it need internet at runtime?** No — only to pull the image, and when metered licensing is enabled.

**Can I run it air-gapped?** Yes: pre-pull the image, use a license file, pin the tag.

**What ports does it open?** None. stdio only.

**What does the index contain?** The documents' extracted text and search data. Protect and dispose of it like the documents.

## Read more

* [3 architectures for AI document processing, and the one that keeps files inside your network](https://blog.groupdocs.com/total/on-premise-ai-document-processing-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

