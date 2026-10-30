---
id: home
url: search/python-net
title: GroupDocs.Search for Python via .NET
weight: 1
description: "Native Python library that indexes documents of more than 80 formats and searches them with keyword, phrase, wildcard, regular-expression, boolean, fuzzy, synonym and faceted queries on Windows, Linux, and macOS. No Microsoft Office or OpenOffice required."
keywords: GroupDocs.Search, Python via .NET, full-text search, document search, search index, fuzzy search, synonym search, highlight search results, PDF, Word, Excel, PowerPoint, email, on-premise, Windows, Linux, macOS, python
productName: GroupDocs.Search for Python via .NET
hideChildren: true
toc: True
structuredData:
    showOrganization: true
---

<img src="/logo/128x128/groupdocs-search-python.png" alt="groupdocs-search-python-net-home" align="left" style="width:110px; margin: 0 30px 30px 0"/>

<img src="https://img.shields.io/pypi/v/groupdocs-search-net?label=GroupDocs.Search%20PyPI" alt="PyPI package">
<img src="https://img.shields.io/pypi/dm/groupdocs-search-net?label=pypi%20downloads" alt="PyPI downloads">

{{< button style="primary" link="https://releases.groupdocs.com/search/python-net/release-notes/" >}} <svg class="gdoc-icon gdoc-product-doc__btn-icon"><use xlink:href="/img/groupdocs-stack.svg#document"></use></svg> Release notes {{< /button >}}
{{< button style="primary" link="https://releases.groupdocs.com/search/python-net/" >}} {{< icon "gdoc_download" >}} Download the wheels {{< /button >}}
{{< button style="primary" link="https://products.groupdocs.app/search/family" >}} <svg class="gdoc-icon gdoc-product-doc__btn-icon"><use xlink:href="/img/groupdocs-stack.svg#app"></use></svg> Online app {{< /button >}}

[GroupDocs.Search for Python via .NET](https://products.groupdocs.com/search/python-net/) is a full-text search API: build an index over a folder of documents, then search it with keyword, phrase, wildcard, regular-expression, boolean, fuzzy, synonym, homophone and faceted queries, and highlight what was found. It indexes the text and metadata of more than 80 formats, including Word processing, spreadsheets, presentations, PDF, email, eBooks, archives and images, with no MS Office or OpenOffice installation required.

<div style="clear:left"></div>

## Quick example

```python
from groupdocs.search import Index

# Create an index in a folder, index a folder of documents, and search it
with Index("./index") as index:
    index.add("./documents")
    result = index.search("relativity")
    print("Documents found:", result.document_count)
```

## Features

- **Persistent index**: Index files, folders and streams into an index on disk or in memory, and reopen it on the next run; `update()` reindexes only the documents that changed.
- **Rich queries**: Keyword, phrase, wildcard, regular-expression, boolean, faceted, date-range and numeric-range search, written as text or built as objects.
- **Find what was meant**: Fuzzy search tolerates typos and OCR noise, and synonym, homophone and word-form search, spell checking and keyboard layout correction widen a query.
- **Highlighting**: Mark the found words in the whole document or in short fragments, as HTML or plain text.
- **Events and Python callbacks**: Follow indexing, skip documents and supply passwords from Python handlers, and plug in your own text extractors, OCR engine and logger.
- **Index maintenance**: Merge, optimize and check indexes, delete and rename documents, and update an index made by an older version.
- **Images**: Reverse image search, and the text of images through an OCR engine you connect.

## Supported File Formats

GroupDocs.Search indexes more than 80 file formats. For a complete list, see the [full list of supported formats]({{< ref "search/python-net/getting-started/supported-document-formats.md" >}}).

- **Word Processing** (DOC, DOCX, DOCM, DOT, DOTX, RTF, ODT, OTT, TXT, MD)
- **Spreadsheets** (XLS, XLSX, XLSM, XLSB, XLTX, ODS, CSV, TSV)
- **Presentations** (PPT, PPTX, PPTM, PPS, PPSX, POT, POTX, ODP)
- **Fixed-Layout** (PDF)
- **Markup** (HTML, HTM, XHTML, MHTML, XML)
- **eBooks** (EPUB, FB2, CHM)
- **Email** (EML, EMLX, MSG, PST, OST)
- **Images** (JPG, PNG, BMP, GIF, TIFF, WEBP, EMF, WMF, PSD, DJVU)
- **Archives** (ZIP)
- **Other** (OneNote, Visio, Microsoft Project, DICOM, MP3, WAV, AVI, MOV, TORRENT)

## Getting Started

To get started, refer to the [System Requirements]({{< ref "search/python-net/getting-started/system-requirements.md" >}}), [Supported Document Formats]({{< ref "search/python-net/getting-started/supported-document-formats.md" >}}), [Installation]({{< ref "search/python-net/getting-started/installation.md" >}}), and [Quick Start Guide]({{< ref "search/python-net/getting-started/quick-start-guide.md" >}}) sections for setup instructions.

## Developer Guide

For practical code examples and explanations of the index-and-search workflow, see the [Developer Guide]({{< ref "search/python-net/developer-guide/_index.md" >}}) section. It covers creating and updating indexes, every search type, search results and highlighting, dictionaries, events and Python callbacks, and search networks.

## AI Agents and LLM Integration

The package ships an `AGENTS.md` reference for AI coding assistants, and the documentation is available to them through an MCP server and as machine-readable text. See [AI agents and LLM integration]({{< ref "search/python-net/agents-and-llm-integration.md" >}}).

## Technical Support

If you experience any issues or have suggestions, please refer to the [Technical Support]({{< ref "search/python-net/technical-support.md" >}}) topic. This topic provides multiple support channels tailored to your needs.
