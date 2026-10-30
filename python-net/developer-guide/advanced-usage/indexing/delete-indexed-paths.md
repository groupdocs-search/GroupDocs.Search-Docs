---
id: delete-indexed-paths
url: search/python-net/delete-indexed-paths
title: Delete indexed paths
weight: 7
description: "GroupDocs.Search supports the ability to remove indexed files and folders from an index. Only files or folders that were explicitly added to the index can be deleted."
keywords: delete indexed paths, remove indexed files, remove indexed folders, get_indexed_paths, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
GroupDocs.Search supports the ability to remove indexed files and folders from an index. Only files or folders that were explicitly added to the index can be deleted. It is not possible to remove a file or folder from the index that is a child of the indexed folder. To delete files and folders inside indexed paths, use the document filter (see [Document filtering during indexing]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}})). To get a list of indexed paths, use the [get_indexed_paths](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_indexed_paths/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class.

Deleting documents does not rewrite the index. Deleted documents are excluded from search results immediately, but their data remain in the index files until the index is [optimized]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}). Therefore, after deleting a large number of documents, call the [optimize](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/optimize/) method to physically remove them and free up disk space. The [DeleteResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/deleteresult/) object returned by the [delete](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/delete/) method contains the total, successful and failed counts, the results for individual items and the error messages.

The following example shows how to remove indexed paths from an index.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import UpdateOptions

documents_folder1 = "./documents"
documents_folder2 = "./documents2"

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folders
index.add(documents_folder1)
index.add(documents_folder2)

# Getting indexed paths from the index
indexed_paths1 = index.get_indexed_paths()

# Writing indexed paths to the console
print("Indexed paths:")
for path in indexed_paths1:
    print("\t" + path)

# Deleting indexed path from the index
delete_result = index.delete([documents_folder1], UpdateOptions())

# Getting indexed paths after deletion
indexed_paths2 = index.get_indexed_paths()
print("\nDeleted paths: " + str(delete_result.success_count))

print("\nIndexed paths:")
for path in indexed_paths2:
    print("\t" + path)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/delete-indexed-paths/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/delete-indexed-paths/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "documents2" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents2/Curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/delete-indexed-paths/documents2/Curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "delete-indexed-paths.txt" >}}  
```text
Indexed paths:
	documents
	documents2

Deleted paths: 1

Indexed paths:
	documents2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/delete-indexed-paths/delete_indexed_paths/delete-indexed-paths.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
