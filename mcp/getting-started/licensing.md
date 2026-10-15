---
id: mcp-licensing
url: search/mcp/getting-started/licensing
title: Licensing
weight: 7
description: "The GroupDocs.Search MCP server runs in evaluation mode out of the box — at most 100 documents per index and 10 per search; unlock it with your GroupDocs.Search license file or metered keys, and check the mode with get_license_status."
keywords: GroupDocs MCP license, MCP server evaluation mode, search only returns 10 results, metered license MCP, GROUPDOCS_METERED_PUBLIC_KEY, get_license_status
productName: GroupDocs.Search MCP Server
toc: True
---

The GroupDocs.Search MCP server runs in **evaluation mode out of the box** — no sign-up, no key. Your existing GroupDocs.Search license unlocks it: mount the `.lic` file into the container and point `GROUPDOCS_LICENSE_PATH` at it.

**The MCP server itself is open source (MIT); the underlying GroupDocs.Search engine requires a license for production use.**

There are three modes. The server takes the first one that is configured:

| Mode | Configure | Result |
|---|---|---|
| **Metered** (pay-per-use) | `GROUPDOCS_METERED_PUBLIC_KEY` **and** `GROUPDOCS_METERED_PRIVATE_KEY` | Billed for what you process; no evaluation limits |
| **License file** | `GROUPDOCS_LICENSE_PATH` | Fully licensed; works offline |
| **Evaluation** | nothing | An index holds at most 100 documents and a search returns at most 10 — every response says so |

{{< alert style="info" >}}
**Metered wins.** If both metered keys and a license file are configured, the server uses metered licensing and ignores the file — and says so in its startup log.
{{< /alert >}}

## Evaluation mode limitations

Without a license:

* An index holds **at most 100 documents**, and a search returns **at most 10 documents**. Results past the cap are not shown — the answer looks complete and is not.
* Every response carries an `evaluation` note while the limits apply, so an agent can tell you.
* Text extracted from some formats (DOCX and PDF in our tests) gains a line *"Evaluation only. Created with GroupDocs.Parser for .NET…"*, and it can appear in [`get_text_fragments`]({{< ref "search/mcp/tools-reference/get-text-fragments.md" >}}) output. After you license the server, re-index with [`create_index`]({{< ref "search/mcp/tools-reference/create-index.md" >}}) (`recreate: true`) and [`add_to_index`]({{< ref "search/mcp/tools-reference/add-to-index.md" >}}) to drop it.
* The library's own evaluation terms: [evaluation limitations and licensing]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md" >}}).

The evaluation note every response carries, verbatim:

```text
Evaluation mode: an index holds at most 100 documents and a search returns
at most 10 documents. Set GROUPDOCS_LICENSE_PATH, or
GROUPDOCS_METERED_PUBLIC_KEY and GROUPDOCS_METERED_PRIVATE_KEY, to lift
both limits.
```

(Wrapped for width.) An **empty** license path is always safe — the server continues in evaluation mode; it never errors because a license is absent.

## Applying a license file

Mount the license folder read-only and point the variable at the file **inside** the container:

```bash
docker run --rm -i -v /path/to/documents:/data \
  -v /path/to/license-folder:/license:ro \
  -e GROUPDOCS_LICENSE_PATH=/license/GroupDocs.Search.lic \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

In a client config, the same three arguments go into `args` before the image name. Your license file is read from local disk by the local container — like your documents, **it never leaves your machine**.

After licensing, rebuild indexes that were built in evaluation mode — [`create_index`]({{< ref "search/mcp/tools-reference/create-index.md" >}}) with `recreate: true`, then [`add_to_index`]({{< ref "search/mcp/tools-reference/add-to-index.md" >}}) — so the evaluation text drops out of the stored content and indexes can grow past 100 documents.

## Metered (pay-per-use) licensing

Metered licensing bills you for what you actually process, which suits AI agents: their usage is bursty and hard to size in advance. **One key pair covers every GroupDocs product and platform.** Forward the two variables to the container **by name** — `-e VAR` with no value copies it from the launching process, so the key never appears in a config file:

```bash
docker run --rm -i -v /path/to/documents:/data \
  -e GROUPDOCS_METERED_PUBLIC_KEY -e GROUPDOCS_METERED_PRIVATE_KEY \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

Set both variables in your OS environment (a stdio server inherits its client's environment). Never commit a literal private key in a project-scoped `.mcp.json`.

{{< alert style="warning" >}}
**Both keys are required, and metered mode needs outbound connectivity.** With only one key set the server stays in evaluation mode and says so. Usage — never document content — is reported to GroupDocs servers, so firewalled deployments must allow that egress or use a license file.
{{< /alert >}}

## Confirming which mode is active

Ask your agent *"is the search server licensed?"*. The [`get_license_status`]({{< ref "search/mcp/tools-reference/get-license-status.md" >}}) tool answers without touching an index — captured from server 26.10.1, unlicensed:

```json
{
  "mode": "evaluation",
  "licensed": false,
  "server": { "name": "GroupDocs.Search.Mcp", "version": "26.10.1" },
  "engine": { "name": "GroupDocs.Search", "version": "26.10" },
  "note": "No license configured — output may carry evaluation limitations. ..."
}
```

If a key pair is rejected, the mode stays `evaluation` and `note` says why — so a mistyped key surfaces here instead of as a search that quietly stops at 10 documents.

## Getting a license

* [Free 30-day temporary license](https://purchase.groupdocs.com/temporary-license/) — full functionality for evaluation.
* [Purchase GroupDocs.Search](https://purchase.groupdocs.com/pricing/search/net) · [Product page](https://products.groupdocs.com/search/net/)
* The library's evaluation terms: [evaluation limitations and licensing]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md" >}})
