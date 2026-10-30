---
id: product-overview
url: search/python-net/product-overview
title: GroupDocs.Search for Python via .NET Overview
linkTitle: Product overview
weight: 1
description: "GroupDocs.Search for Python via .NET indexes documents of more than 80 formats and searches them with keyword, phrase, wildcard, regular-expression, boolean, fuzzy, synonym, homophone and faceted queries, with highlighting, events and Python callbacks, on-premise, with no Microsoft Office required."
keywords: full-text search, document search, search index, fuzzy search, synonym search, highlight search results, PDF, DOCX, XLSX, PPTX, email, python
productName: GroupDocs.Search for Python via .NET
toc: True
aliases:
    - /search/python-net/groupdocs-search-overview/
structuredData:
  showOrganization: True
  application:
    name: Document Search Tool
    description: The product indexes documents of more than 80 formats and searches them with simple, fuzzy, boolean, phrase, faceted, wildcard, regular expression and other search types, and highlights the search results in the text of documents.
    productCode: search
    productPlatform: python-net
---

## What is GroupDocs.Search?

GroupDocs.Search for Python via .NET is a native Python library for [full-text search](https://en.wikipedia.org/wiki/Full-text_search) across documents of more than 80 formats. It extracts the text and metadata of PDF, Word, Excel, PowerPoint, OpenDocument, email, eBook, archive and image files into an index on your own disk, so that a search over thousands of documents takes moments. It runs entirely on-premise, requires no Microsoft Office installation, and ships as a pre-built wheel for Windows, Linux, and macOS.

Typical uses include:

- **Search inside an application** - let users find documents by their content, with queries that tolerate typos and know synonyms.
- **Retrieval for AI pipelines** - find the documents and the passages an LLM needs, locally; see [AI agents and LLM integration]({{< ref "search/python-net/agents-and-llm-integration.md" >}}).
- **E-discovery and compliance** - find every document that mentions a name or a contract number, or that matches a pattern such as a date or an account number.
- **Archive and knowledge-base search** - index mail archives, ZIP files and the text of scans, and keep the index current as files are added, changed and deleted.
- **Image search** - find the documents that contain a given picture with reverse image search.

## Key Capabilities

| Capability | Description |
|---|---|
| **Indexing** | [Create an index]({{< ref "search/python-net/developer-guide/basic-usage/index-lifecycle.md" >}}) in a folder or in memory, add files, folders and streams, and [update]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md" >}}) it with only the documents that changed. |
| **Search types** | [Keyword, phrase, wildcard, regular-expression and boolean queries]({{< ref "search/python-net/developer-guide/basic-usage/build-search-query.md" >}}), and [faceted]({{< ref "search/python-net/developer-guide/advanced-usage/searching/faceted-search.md" >}}), [date range]({{< ref "search/python-net/developer-guide/advanced-usage/searching/date-range-search.md" >}}) and [numeric range]({{< ref "search/python-net/developer-guide/advanced-usage/searching/numeric-range-search.md" >}}) search. |
| **Flexible matching** | [Fuzzy]({{< ref "search/python-net/developer-guide/advanced-usage/searching/fuzzy-search.md" >}}), [synonym]({{< ref "search/python-net/developer-guide/advanced-usage/searching/synonym-search.md" >}}), [homophone]({{< ref "search/python-net/developer-guide/advanced-usage/searching/homophone-search.md" >}}) and [word-form]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}) search, [spell checking]({{< ref "search/python-net/developer-guide/advanced-usage/searching/spell-checking.md" >}}) and [keyboard layout correction]({{< ref "search/python-net/developer-guide/advanced-usage/searching/keyboard-layout-correction.md" >}}). |
| **Highlighting** | [Mark the found words]({{< ref "search/python-net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}) in the whole document or in fragments, as HTML or plain text. |
| **Search results** | [Found documents, occurrences and found fields]({{< ref "search/python-net/developer-guide/basic-usage/work-with-search-results.md" >}}), [search reports]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-reports.md" >}}) and [search by chunks]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}). |
| **Index maintenance** | [Merge]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/merge-indexes.md" >}}), [optimize]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}) and [check]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}}) indexes, [delete documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/delete-indexed-documents.md" >}}), and [update an index made by an older version]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}). |
| **Dictionaries** | [Synonyms]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/synonym-dictionary.md" >}}), [homophones]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/homophone-dictionary.md" >}}), [stop words]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary.md" >}}), [aliases]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/alias-dictionary.md" >}}), [document passwords]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/document-passwords.md" >}}) and more, read and changed from Python. |
| **Events** | [Follow indexing and search]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}) with handlers such as `index.events.file_indexing += handler`, skip documents, and supply passwords as documents need them. |
| **Python callbacks** | Write [text extractors]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}), [OCR connectors]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/ocr-support.md" >}}), [loggers]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/logging.md" >}}) and [word form providers]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/word-forms-provider.md" >}}) as plain Python classes or functions. |
| **Images** | [Reverse image search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/reverse-image-search.md" >}}), and the text of images through an [OCR engine]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/ocr-support.md" >}}) you connect. |
| **Scaling** | A [search network]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/_index.md" >}}) of shards that shares indexing and search across nodes. |
| **On-premise** | No cloud calls, no Microsoft Office install, no network traffic. |

## Quick Example

Index a folder of documents in different formats and search it with just a few lines of code:

{{< tabs "product-overview-quick-example" >}}
{{< tab "quick_example.py" >}}
```python
import os

from groupdocs.search import Index

def quick_example():
    # Create an index in the "index" folder, or open the one already there
    with Index("./index") as index:
        # Index every supported document in the "documents" folder
        index.add("./documents")

        # Search the index and list the documents found
        result = index.search("relativity")
        print("Documents found:", result.document_count)
        # Most relevant first: the result lists the documents in the order they were indexed
        for document in sorted(result, key=lambda found: found.relevance, reverse=True):
            print(os.path.basename(document.document_info.file_path), "-", document.occurrence_count, "occurrences")

if __name__ == "__main__":
    quick_example()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.docx](/search/python-net/_sample_files/product-overview/documents/einstein-biography.docx), [documents/einstein-summary.html](/search/python-net/_sample_files/product-overview/documents/einstein-summary.html), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/product-overview/documents/nobel-prize-1921.txt), [documents/relativity-guide.epub](/search/python-net/_sample_files/product-overview/documents/relativity-guide.epub), [documents/relativity-notes.pdf](/search/python-net/_sample_files/product-overview/documents/relativity-notes.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "quick.txt" >}}  
```text
Documents found: 2
relativity-notes.pdf - 5 occurrences
relativity-guide.epub - 5 occurrences
```
[Download full output](/search/python-net/_output_files/product-overview/quick_example/quick.txt)
{{< /tab >}}
{{< /tabs >}}

For a query with a typo, turn on fuzzy search:

{{< tabs "product-overview-search-with-options" >}}
{{< tab "search_with_options.py" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchOptions

def search_with_options():
    with Index("./index") as index:
        index.add("./documents")

        # Fuzzy search also finds words that differ from the query by a few characters
        options = SearchOptions()
        options.fuzzy_search.enabled = True

        result = index.search("Einstien", options)
        print("Documents found:", result.document_count)
        for document in sorted(result, key=lambda found: found.relevance, reverse=True):
            print(os.path.basename(document.document_info.file_path), "-", document.occurrence_count, "occurrences")

if __name__ == "__main__":
    search_with_options()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.docx](/search/python-net/_sample_files/product-overview/documents/einstein-biography.docx), [documents/einstein-summary.html](/search/python-net/_sample_files/product-overview/documents/einstein-summary.html), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/product-overview/documents/nobel-prize-1921.txt), [documents/relativity-guide.epub](/search/python-net/_sample_files/product-overview/documents/relativity-guide.epub), [documents/relativity-notes.pdf](/search/python-net/_sample_files/product-overview/documents/relativity-notes.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-options.txt" >}}  
```text
Documents found: 4
einstein-summary.html - 2 occurrences
relativity-notes.pdf - 2 occurrences
einstein-biography.docx - 2 occurrences
nobel-prize-1921.txt - 2 occurrences
```
[Download full output](/search/python-net/_output_files/product-overview/search_with_options/search-options.txt)
{{< /tab >}}
{{< /tabs >}}

## Where to next

1. **Install the package** - [Installation]({{< ref "search/python-net/getting-started/installation.md" >}}) walks through installing the wheel on Windows, Linux, and macOS.
2. **Run your first search** - the [Quick Start Guide]({{< ref "search/python-net/getting-started/quick-start-guide.md" >}}) indexes and searches a folder of documents in five minutes.
3. **Explore the examples** - [How to Run Examples]({{< ref "search/python-net/getting-started/how-to-run-examples.md" >}}) clones the runnable repository and runs every documented scenario locally or in Docker.
4. **Use it in depth** - the [Developer Guide]({{< ref "search/python-net/developer-guide/_index.md" >}}) covers indexing, every search type, highlighting, dictionaries, events and search networks.
5. **Licensing** - [Licensing and evaluation]({{< ref "search/python-net/getting-started/licensing-and-subscription.md" >}}) explains the evaluation limits and how to apply a license.
