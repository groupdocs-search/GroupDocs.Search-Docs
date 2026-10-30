---
id: storing-text-of-indexed-documents
url: search/python-net/storing-text-of-indexed-documents
title: Storing text of indexed documents
weight: 24
description: "This article explains how to store the text of indexed documents in an index."
keywords: store text of indexed documents, Text extracted from indexed documents, TextStorageSettings, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Text extracted from indexed documents can be stored in an index to provide the extracted text to the user faster when the [get_document_text](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_document_text/) method is called, as well as to accelerate text generation with highlighting of search results.

To specify storage parameters, use the [text_storage_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/text_storage_settings/) property of the [IndexSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/) class. The default value is `None`, which means that the text of the documents is not stored in the index. The setting is fixed when the index is created: it cannot be changed for an existing index.

When saving text in the index, the [compression](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textstoragesettings/compression/) property is used to specify the compression ratio of the saved text. Compression can be normal, high, or text can be saved without compression (the `Compression.NORMAL`, `Compression.HIGH` and `Compression.NONE` values). The choice of compression ratio affects the final size of the index, as well as the speed of indexing. A high compression ratio reduces index size and indexing speed, and the lack of compression makes index size and indexing speed maximum. The default compression ratio is normal.

The example below demonstrates storing text in an index using the high compression ratio.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings
from groupdocs.search.options import Compression, TextStorageSettings

# Creating an index settings instance
settings = IndexSettings()
settings.text_storage_settings = TextStorageSettings(Compression.HIGH)  # Setting high compression ratio for the index text storage

# Creating a new index in the specified folder
index = Index("./index", settings, True)

# Indexing documents
index.add("./documents")

# Searching
result = index.search("Einstein")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "storing-text-of-indexed-documents.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents/storing_text_of_indexed_documents/storing-text-of-indexed-documents.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
