---
id: search-index-repository
url: search/python-net/search-index-repository
title: Search index repository
weight: 2
description: "The IndexRepository class can be used to combine several indexes into a group for searching all indexes at once in GroupDocs.Search for Python via .NET."
keywords: combine several indexes, searching all indexes at once, searching all indexes, combine several indexes into a group
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The [IndexRepository](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexrepository/) class can be used to combine several indexes into a group for searching all indexes at once. In addition, a single call to the [update](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexrepository/update/) method can update all indexes in the repository. The index repository also serves as an event hub for all indexes in it: you can subscribe to any events available for an index also through the index repository. However, note that indexing documents is performed separately for each index.

To start using the index repository, indexes must be created or loaded and added to an instance of the index repository with the [add_to_repository](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexrepository/add_to_repository/) method, or created directly in the repository with the [create](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexrepository/create/) method. The [indexes](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexrepository/indexes/) property returns the indexes of the repository. Examples of creating and adding indexes to the repository are presented below.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexRepository

# Creating an index repository instance
index_repository = IndexRepository()

# Creating or loading an index and adding to the index repository
index1 = Index("./index1")
index_repository.add_to_repository(index1)

# Creating an index via the index repository by calling a single method
index2 = index_repository.create("./index2")

# Adding documents to the index 1
index1.add("./documents1")

# Adding documents to the index 2
index2.add("./documents2")

print("Indexes in the repository:", len(index_repository.indexes))
```
{{< /tab >}}
{{< tab "documents1" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents1/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/documents1/einstein-notes.txt), [documents1/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/documents1/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "documents2" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents2/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/documents2/newton.txt), [documents2/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/documents2/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-index-repository.txt" >}}  
```text
Indexes in the repository: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/search_index_repository/search-index-repository.txt)
{{< /tab >}}
{{< /tabs >}}

The following example shows how to subscribe to the repository events, update indexes in the repository, and search in the repository.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexRepository

# Creating an index repository instance
index_repository = IndexRepository()


# Subscribing to an event
def on_error_occurred(sender, args):
    print("Index:", args.index_folder)
    print("Error:", args.message)


index_repository.events.error_occurred += on_error_occurred

# Creating or loading an index and adding to the index repository
index1 = Index("./index1")
index_repository.add_to_repository(index1)

# Creating or loading an index and adding to the index repository
index2 = Index("./index2")
index_repository.add_to_repository(index2)

# Adding documents to the index 1
index1.add("./documents1")

# Adding documents to the index 2
index2.add("./documents2")

# Changing, deleting, adding documents to document folders
# ...

# Updating all indexes in the repository
index_repository.update()

# Searching in the repository
result = index_repository.search("Einstein")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents1" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents1/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/documents1/einstein-notes.txt), [documents1/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/documents1/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "documents2" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents2/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/documents2/newton.txt), [documents2/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/documents2/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-index-repository-2.txt" >}}  
```text
Index: index2
Error: Error during text extraction from documents2\damaged.pdf
GroupDocs.Parser.Exceptions.UnsupportedDocumentFormatException: Exception of type 'GroupDocs.Parser.Exceptions.UnsupportedDocumentFormatException' was thrown.
   at .(Stream , LoadOptions )
   at .()
   at GroupDocs.Parser.Parser.GetMetadata()
   at .()
   at .(Document ,  , String )
Index: index2
Error: Error during text extraction from documents2\damaged.pdf
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-repository/search_index_repository_2/search-index-repository-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
