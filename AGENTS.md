# GroupDocs.Search Docs - AGENTS.md

> Instructions for AI agents working with **GroupDocs.Search** documentation in this repository.

GroupDocs.Search is an on-premise SDK for full-text search: it builds search indexes from documents in PDF, Microsoft Office, OpenDocument, email, eBook, archive, image and other formats, and runs keyword, phrase, wildcard, regular-expression, fuzzy, boolean, faceted, synonym and homophone queries against them.

**Supported formats (canon):** the per-platform tables below. Format lists differ between platforms, so do not quote one platform's list or count for another.

## Platforms in this docs tree

| Platform slug | Docs hub | Formats table |
|---|---|---|
| `net` | [net/_index.md](net/_index.md) | [net/getting-started/supported-document-formats.md](net/getting-started/supported-document-formats.md) |
| `java` | [java/_index.md](java/_index.md) | [java/getting-started/supported-document-formats.md](java/getting-started/supported-document-formats.md) |
| `nodejs-java` | [nodejs-java/_index.md](nodejs-java/_index.md) | [nodejs-java/getting-started/supported-document-formats.md](nodejs-java/getting-started/supported-document-formats.md) |
| `python-net` | [python-net/_index.md](python-net/_index.md) | [python-net/getting-started/supported-document-formats.md](python-net/getting-started/supported-document-formats.md) |
| `mcp` | [mcp/_index.md](mcp/_index.md) | [mcp/supported-formats.md](mcp/supported-formats.md) |

## .NET target frameworks (canon)

When documenting .NET install/runtime requirements, quote [net/getting-started/system-requirements.md](net/getting-started/system-requirements.md) and do not invent other TFMs. The GroupDocs.Search NuGet package (26.10.0) declares a single target framework, **netstandard2.1**; that page also lists a .NET Framework 4.6.2 build.

The Python package (`groupdocs-search-net`) embeds its own .NET runtime, so Python readers install no .NET at all. Its requirements are on the python-net system requirements page.

## Product lines (do not confuse)

| Line | Where to send readers |
|---|---|
| **On-premise SDK** (this docs tree) | Platform hubs above; examples repos; NuGet / Maven / PyPI / npm packages |
| **GroupDocs.Search MCP server** (this docs tree, `mcp/` and `net/mcp/`) | An MCP server that indexes a folder and searches it for AI agents, shipped as a Docker image |
| **GroupDocs Cloud** | Separate Cloud product docs/packages - do not mix install snippets |
| **Docs MCP** | Link only: [https://docs.groupdocs.com/mcp](https://docs.groupdocs.com/mcp) - do not edit MCP repos from this surface |

## Editing conventions

- The public URL of a page comes from its `url:` front matter, not from its file path. Keep `url:` unchanged when you move or rename a file.
- `{{< ref "..." >}}` resolves content paths: after moving a file, update every `ref` that points to it.
- Redirects are Hugo `aliases:` front matter on the destination page.
- Menu titles (`title:`, `linkTitle:`) are plain text: no emoji.
- Code examples on a page have runnable counterparts in the platform's examples repository; keep the two in sync.
- The python-net code examples were run against the `groupdocs-search-net` wheel they describe; change one only together with a run of it.

## Useful entry URLs

| Resource | URL |
|---|---|
| Docs home | [https://docs.groupdocs.com/search/](https://docs.groupdocs.com/search/) |
| Products | [https://products.groupdocs.com/search/](https://products.groupdocs.com/search/) |
| Releases | [https://releases.groupdocs.com/search/](https://releases.groupdocs.com/search/) |
| API reference | [https://reference.groupdocs.com/search/](https://reference.groupdocs.com/search/) |
| Examples (.NET) | [https://github.com/groupdocs-search/GroupDocs.Search-for-.NET](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET) |
| Examples (Java) | [https://github.com/groupdocs-search/GroupDocs.Search-for-Java](https://github.com/groupdocs-search/GroupDocs.Search-for-Java) |
| Examples (Node.js) | [https://github.com/groupdocs-search/GroupDocs.Search-for-Node.js-via-Java](https://github.com/groupdocs-search/GroupDocs.Search-for-Node.js-via-Java) |
| Examples (Python) | [https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET) |
| MCP server | [https://github.com/groupdocs-search/GroupDocs.Search.Mcp](https://github.com/groupdocs-search/GroupDocs.Search.Mcp) |

Also see [llms.txt](llms.txt) in this repository root for a compact machine-oriented index.
