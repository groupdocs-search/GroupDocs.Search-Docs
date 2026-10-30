---
id: delete-indexed-documents
url: search/python-net/delete-indexed-documents
title: Delete indexed documents
weight: 6
description: "This article explains how to delete documents indexed from streams or structures from an index by their document keys."
keywords: delete documents, remove documents from index, document key, DeleteResult, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---

GroupDocs.Search has the ability to remove individual documents from the index that are indexed from a stream or structure. Such documents are identified by their document keys. To remove documents indexed by a file system path, see [Delete indexed paths]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/delete-indexed-paths.md" >}}).

Deleting documents does not rewrite the index. Deleted documents are excluded from search results immediately, but their data remain in the index files until the index is [optimized]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}). Therefore, after deleting a large number of documents, call the [optimize](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/optimize/) method to physically remove them and free up disk space. The [DeleteResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/deleteresult/) object returned by the [delete](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/delete/) method contains the total, successful and failed counts, the results for individual items and the error messages.

The following example shows how to remove documents from an index by document key. The document is indexed from a stream with a document loader (see [Indexing from different sources]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-from-different-sources.md" >}})); in Python, the loader is a plain class with the `load_document` and `close_document` methods.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import datetime
import io
import os

from groupdocs.search import Index
from groupdocs.search.common import Document, DocumentSourceKind
from groupdocs.search.options import IndexingOptions, UpdateOptions


# Implementing document loader
class DocumentLoader(object):
    def __init__(self, file_path):
        self.file_path = file_path
        self.document_key = os.path.basename(file_path)

    def close_document(self):
        pass

    def load_document(self):
        extension = os.path.splitext(self.file_path)[1]
        with open(self.file_path, "rb") as f:
            buffer = f.read()
        stream = io.BytesIO(buffer)
        document = Document.create_from_stream(self.document_key, datetime.datetime.now(), extension, stream)
        return document


file_path = "./sample.pdf"

# Creating an index in the specified folder
index = Index("./index")

# Indexing the document from stream
document_loader = DocumentLoader(file_path)
document = Document.create_lazy(DocumentSourceKind.STREAM, document_loader.document_key, document_loader)
documents = [document]
index.add(documents, IndexingOptions())

# Searching in the index
search_result = index.search("Einstein")
print("Documents found: " + str(search_result.document_count))

# Deleting indexed document from the index
document_keys = [document_loader.document_key]
delete_result = index.delete(UpdateOptions(), document_keys)
print("Deleted documents: " + str(delete_result.success_count))

# Physically removing the deleted documents from the index files
index.optimize()
```
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/delete-indexed-documents/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "delete-indexed-documents.txt" >}}  
```text
Documents found: 1
Deleted documents: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/delete-indexed-documents/delete_indexed_documents/delete-indexed-documents.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
