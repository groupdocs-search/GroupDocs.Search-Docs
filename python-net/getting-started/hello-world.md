---
id: hello-world
url: search/python-net/hello-world
title: Hello, World!
second_title: Basic Example of Using GroupDocs.Search for Python via .NET
linkTitle: Hello World
weight: 2
keywords: "hello world, example, get started, index, search"
description: Quickly get started with GroupDocs.Search for Python via .NET by creating and running a simple example that indexes a folder of documents and searches it.
productName: GroupDocs.Search for Python via .NET
hideChildren: False
toc: True
---

## Introduction
A "Hello, World!" example is often the first step when exploring GroupDocs.Search for Python via .NET. It serves as a simple test to confirm that your development environment is correctly set up and that the library is functioning as expected.

## Overview
GroupDocs.Search for Python via .NET indexes documents and searches them by their content. A wide range of [supported formats]({{< ref "search/python-net/getting-started/supported-document-formats.md" >}}) makes it versatile for different use cases: one index can hold Word documents, spreadsheets, PDF files, emails and many more.

## How to index and search documents
The following steps demonstrate how to index a folder of documents and search it using GroupDocs.Search for Python via .NET:

1. Import the `Index` class from `groupdocs.search`.
2. Create an `Index` in a folder of its own. The index is kept there, and opened again on the next run.
3. Add the folder of documents to the index with `add()`.
4. Search the index with `search()`.
5. Read the number of documents found and the occurrences in each, the most relevant document first.

## Complete example
The example below indexes the documents of the `documents` folder and searches them for the word "physics":

{{< tabs "code-example-hello-world" >}}
{{< tab "hello_world.py" >}}
```python
import os

from groupdocs.search import Index

def hello_world():
    # Create an index in the "index" folder, or open the one already there
    with Index("./index") as index:
        # Index every supported document in the "documents" folder
        index.add("./documents")

        # Search the index for a word
        result = index.search("physics")

        print("Documents found:", result.document_count)
        # Most relevant first: the result lists the documents in the order they were indexed
        for document in sorted(result, key=lambda found: found.relevance, reverse=True):
            print(os.path.basename(document.document_info.file_path), "-", document.occurrence_count, "occurrences")

if __name__ == "__main__":
    hello_world()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/getting-started/hello-world/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/getting-started/hello-world/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/getting-started/hello-world/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/getting-started/hello-world/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "hello-world.txt" >}}  
```text
Documents found: 2
einstein.txt - 2 occurrences
nobel-prize-1921.txt - 1 occurrences
```
[Download full output](/search/python-net/_output_files/getting-started/hello-world/hello_world/hello-world.txt)
{{< /tab >}}
{{< /tabs >}}

The documents are sorted by [relevance](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/relevance/) because the result lists them in the order they were indexed, which is the order in which the file system lists the folder. That is name order on Windows but not on Linux, so without sorting the two documents can come the other way round there.

Run the script again and the index opens from the `index` folder: `add()` indexes only the documents that are new or have changed since the last run. To search other documents, put them into the `documents` folder, or pass another folder to `add()`.

## Additional resources
This demo references the GroupDocs.Search for Python via .NET [code samples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET/).
