---
id: indexing-metadata-of-documents
url: search/python-net/indexing-metadata-of-documents
title: Indexing metadata of documents
weight: 14
description: "This article explains how to create an index that contains only the metadata of documents by setting IndexType.METADATA_INDEX when creating the index in Python."
keywords: metadata index, index only metadata, IndexType, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
GroupDocs.Search allows creating a full-text index or a metadata index of documents. To index only metadata without the main content of documents, set the [index_type](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/index_type/) property of the index settings to [IndexType.METADATA_INDEX](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indextype/) when creating an index. The size of an index of this type will be very small. The index type is fixed when the index is created, so it cannot be changed for an existing index.

The following example demonstrates the creation of the metadata index.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings
from groupdocs.search.options import IndexType

# Creating an instance of index settings
settings = IndexSettings()
settings.index_type = IndexType.METADATA_INDEX  # Setting index type

# Creating a new index in the specified folder
index = Index("./index", settings, True)

# Indexing documents from the specified folder
index.add("./documents")

# Searching in the index
result = index.search("Einstein")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Lecture.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-metadata-of-documents/documents/Lecture.docx), [documents/Biography.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-metadata-of-documents/documents/Biography.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-metadata-of-documents.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-metadata-of-documents/indexing_metadata_of_documents/indexing-metadata-of-documents.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
