---
id: quick-start-guide
url: search/python-net/quick-start-guide
title: Quick Start Guide
linkTitle: Quick Start Guide
second_title: A simple example of how to use GroupDocs.Search for Python via .NET
weight: 4
keywords: quick start, hello world, get started, first search, index documents, fuzzy search, highlight, pip install, venv, virtual environment, GroupDocs.Search, python
description: "Set up a virtual environment, install groupdocs-search-net, and run three minimal examples - search an indexed folder, search with a typo, and highlight what was found - in under five minutes."
productName: GroupDocs.Search for Python via .NET
hideChildren: False
toc: True
---

This guide provides a quick overview of how to set up and start using GroupDocs.Search for Python via .NET. The library builds an index over your documents, searches it with a rich query language, and shows where in each document the found words are.

## Prerequisites

To proceed, make sure you have:

1. **Configured** environment as described in the [System Requirements]({{< ref "search/python-net/getting-started/system-requirements.md" >}}) topic.
2. **Optionally** you may [Get a Temporary License](https://purchase.groupdocs.com/temporary-license/) to test all the product features.

## Set Up Your Development Environment

For best practices, use a virtual environment to manage dependencies in Python applications. Learn more about virtual environments at the [Create and Use Virtual Environments](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#create-and-use-virtual-environments) documentation topic.

### Create and Activate a Virtual Environment

Create a virtual environment:

{{< tabs "example1">}}
{{< tab "Windows" >}}
```ps
py -m venv .venv
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m venv .venv
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m venv .venv
```
{{< /tab >}}
{{< /tabs >}}

Activate a virtual environment:

{{< tabs "example2">}}
{{< tab "Windows" >}}
```ps
.venv\Scripts\activate
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
source .venv/bin/activate
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
source .venv/bin/activate
```
{{< /tab >}}
{{< /tabs >}}

### Install `groupdocs-search-net` Package

After activating the virtual environment, run the following command in your terminal to install the latest version of the package:

{{< tabs "example3">}}
{{< tab "Windows" >}}
```ps
py -m pip install groupdocs-search-net
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m pip install groupdocs-search-net
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m pip install groupdocs-search-net
```
{{< /tab >}}
{{< /tabs >}}

Ensure the package is installed successfully. You should see the message

```bash
Successfully installed groupdocs-search-net-*
```

{{< alert style="info" >}}
Until PyPI carries the 26.10 wheels, install the wheel for your platform from the [GroupDocs Releases website](https://releases.groupdocs.com/search/python-net/) instead, as described in [Install from a Pre-Downloaded Wheel]({{< ref "search/python-net/getting-started/installation.md#install-from-a-pre-downloaded-wheel" >}}).
{{< /alert >}}

## Example 1: Index and search documents

To quickly test the library, let's index a folder of documents and search it for a phrase.

{{< tabs "demo_app_search_documents">}}
{{< tab "search_documents.py" >}}
```python
import os

from groupdocs.search import Index, License

def search_documents():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Search.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Create an index in the "index" folder and index the "documents" folder
    with Index("./index") as index:
        index.add("./documents")

        # A phrase in double quotes matches these words in this order
        result = index.search('"theory of relativity"')

        print("Documents found:", result.document_count)
        print("Occurrences found:", result.occurrence_count)
        # Most relevant first: the result lists the documents in the order they were indexed
        for document in sorted(result, key=lambda found: found.relevance, reverse=True):
            print(os.path.basename(document.document_info.file_path), "-", document.occurrence_count, "occurrences")

if __name__ == "__main__":
    search_documents()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-documents.txt" >}}  
```text
Documents found: 2
Occurrences found: 3
einstein.txt - 2 occurrences
relativity.txt - 1 occurrences
```
[Download full output](/search/python-net/_output_files/getting-started/quick-start-guide/search_documents/search-documents.txt)
{{< /tab >}}
{{< /tabs >}}

Your folder tree should look similar to the following directory structure:

```Directory
📂 demo-app
 ├──search_documents.py
 ├──📂 documents
 │   ├──einstein.txt
 │   ├──nobel-prize-1921.txt
 │   ├──relativity.txt
 │   └──time.txt
 └──GroupDocs.Search.lic (Optionally)
```

### Run the App

{{< tabs "run_the_app_search_documents">}}
{{< tab "Windows" >}}
```ps
py search_documents.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 search_documents.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 search_documents.py
```
{{< /tab >}}
{{< /tabs >}}

The first run creates the `index` folder next to the script. Later runs open the index from there, and `add()` indexes only the documents that are new or have changed. After running the app you can deactivate the virtual environment by executing `deactivate` or closing your shell.

### Explanation
- `Index("./index")`: Creates an index in the `index` folder, or opens the index already stored there. Used as a context manager so the index is closed when the block ends.
- `index.add("./documents")`: Indexes every document of a supported format in the folder and its subfolders.
- `index.search('"theory of relativity"')`: Runs a query. The double quotes make it a phrase: the words must appear together and in this order.
- `result.document_count` / `result.occurrence_count`: The numbers of documents found and of occurrences in all of them. Iterating over `result` gives each found document, in the order the documents were indexed, which follows the file system: name order on Windows, but not on Linux. Sorting by `relevance` lists the most relevant first on every system.

## Example 2: Search with a typo

In this example we search for a misspelled name. Fuzzy search finds the words that differ from the query by a few characters.

{{< tabs "demo_app_fuzzy_search">}}
{{< tab "fuzzy_search.py" >}}
```python
import os

from groupdocs.search import Index, License
from groupdocs.search.options import SearchOptions

def fuzzy_search():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Search.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Index("./index") as index:
        index.add("./documents")

        # Fuzzy search also finds words that differ from the query by a few characters
        options = SearchOptions()
        options.fuzzy_search.enabled = True

        result = index.search("Einstien", options)
        print("Documents found:", result.document_count)
        for document in sorted(result, key=lambda found: found.relevance, reverse=True):
            # The words of the document that matched the query
            words = set()
            for field in document.found_fields:
                words.update(field.terms)
            print(os.path.basename(document.document_info.file_path), "-", ", ".join(sorted(words)))

if __name__ == "__main__":
    fuzzy_search()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "fuzzy-search.txt" >}}  
```text
Documents found: 2
einstein.txt - einstein
nobel-prize-1921.txt - einstein
```
[Download full output](/search/python-net/_output_files/getting-started/quick-start-guide/fuzzy_search/fuzzy-search.txt)
{{< /tab >}}
{{< /tabs >}}

### Run the App

{{< tabs "run_the_app_fuzzy_search">}}
{{< tab "Windows" >}}
```ps
py fuzzy_search.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 fuzzy_search.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 fuzzy_search.py
```
{{< /tab >}}
{{< /tabs >}}

### Explanation
- `SearchOptions()`: Holds the settings of one search. `fuzzy_search.enabled = True` turns on fuzzy search with its default similarity level.
- `index.search("Einstien", options)`: Searches with these options. Without them, the misspelled query finds nothing.
- `document.found_fields`: The fields of a found document that matched, such as `content` and `filename`, each with the words it found in `terms`.

## Example 3: Highlight the found words

Sometimes you need to show where in a document a word was found. A fragment highlighter returns the found words with a few words of context around each.

{{< tabs "demo_app_highlight_results">}}
{{< tab "highlight_results.py" >}}
```python
import os

from groupdocs.search import Index, License
from groupdocs.search.highlighters import FragmentHighlighter
from groupdocs.search.options import HighlightOptions, OutputFormat

def highlight_results():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Search.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Index("./index") as index:
        index.add("./documents")
        result = index.search("gravity")

        # Four words of context on each side of a found word, which is put in brackets
        options = HighlightOptions()
        options.terms_before = 4
        options.terms_after = 4
        options.term_highlight_start_tag = "["
        options.term_highlight_end_tag = "]"

        for document in sorted(result, key=lambda found: found.relevance, reverse=True):
            highlighter = FragmentHighlighter(OutputFormat.PLAIN_TEXT)
            index.highlight(document, highlighter, options)
            print(os.path.basename(document.document_info.file_path))
            for container in highlighter.get_result():
                for fragment in container.get_fragments():
                    print("   ", " ".join(fragment.split()))

if __name__ == "__main__":
    highlight_results()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/getting-started/quick-start-guide/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "highlight-results.txt" >}}  
```text
relativity.txt
    in the absence of [gravity]. General relativity explains the
einstein.txt
    theory of relativity described [gravity] as the curvature of
```
[Download full output](/search/python-net/_output_files/getting-started/quick-start-guide/highlight_results/highlight-results.txt)
{{< /tab >}}
{{< /tabs >}}

### Run the App

{{< tabs "run_the_app_highlight_results">}}
{{< tab "Windows" >}}
```ps
py highlight_results.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 highlight_results.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 highlight_results.py
```
{{< /tab >}}
{{< /tabs >}}

### Explanation
- `HighlightOptions()`: How many words of context to keep before and after each found word (`terms_before`, `terms_after`), and the tags put around the found words.
- `FragmentHighlighter(OutputFormat.PLAIN_TEXT)`: Collects the fragments as plain text. `OutputFormat.HTML` marks the found words with HTML instead.
- `index.highlight(document, highlighter, options)`: Highlights the found words of one found document. `get_result()` then gives the fragments of each field.

## Next Steps

After completing the basics, explore additional resources to enhance your usage:
- [Supported Document Formats]({{< ref "search/python-net/getting-started/supported-document-formats.md" >}}): Review the full list of supported file types.
- [Licensing and Evaluation]({{< ref "search/python-net/getting-started/licensing-and-subscription.md" >}}): Check details on licensing and the evaluation limits.
- [Developer Guide]({{< ref "search/python-net/developer-guide/_index.md" >}}): Dive into indexing, every search type, highlighting, dictionaries and index events.
- [Technical Support]({{< ref "search/python-net/technical-support.md" >}}): Contact support for assistance if you encounter issues.
