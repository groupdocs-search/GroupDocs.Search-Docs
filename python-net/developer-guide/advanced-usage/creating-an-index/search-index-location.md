---
id: search-index-location
url: search/python-net/search-index-location
title: Search index location
weight: 0
description: "This page describes how to create an index on disk or in memory, how to load an index into memory and how to save an index to another folder with GroupDocs.Search for Python via .NET."
keywords: index location, in-memory index, index on disk, on-disk index
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page describes how to create an index on disk or in memory, how to load an index into memory and how to save an index to another folder.

The GroupDocs.Search for Python via .NET search index can be created and stored on disk or entirely in RAM. All changes to the on-disk index are automatically saved and available after it is loaded again. The in-memory index is faster, but uses more RAM and is completely destroyed when you finish working with it.

To create an in-memory index, you must use a constructor that does not have a path to the index folder in its parameter list. The created in-memory index can be converted to an on-disk index using the [save_to](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/save_to/) method. This is useful when an in-memory index becomes too large and can no longer reside in RAM alone. After saving to disk, the index can then be loaded from disk as usual. Note that a regular index can be saved to disk in a different folder in the same way. Below is an example of creating an in-memory index and converting it to an on-disk index.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an in-memory index
index = Index()

# Indexing documents from the specified folder
index.add("./documents")

# Saving the index to disk
index.save_to("./index")

# Closing the in-memory index
index.dispose()

# Opening the index from disk
index = Index("./index")

# Searching in the index
query = "focus"
result = index.search(query)
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-location/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-location/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-location/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-index-location.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-location/search_index_location/search-index-location.txt)
{{< /tab >}}
{{< /tabs >}}

The [save_to](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/save_to/) method follows these rules:

*   It raises `RuntimeError` if the specified folder is the folder of the index itself, also when the folder is specified by another path.
*   An index of the current version in the specified folder is replaced by the saved index.
*   It raises `RuntimeError` and does not change the folder if the folder contains an index of another version (for example, an index created by a previous version) or a damaged index.

For details, see the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

To create or load an on-disk index, you must pass the path to the index folder through the constructor parameter. The created on-disk index can be converted to an in-memory index using the [load_into_memory_completely](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/load_into_memory_completely/) method. After the index is completely loaded into memory, no changes to it are automatically saved to disk. You must explicitly call the [save_to](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/save_to/) method to save any changes to the index. The overload `load_into_memory_completely(index_folder, settings)` allows you to pass an index settings object: the loaded index uses the settings saved in it and takes only the [custom extractors](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/custom_extractors/) and the [logger](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/logger/) from the passed object, since they are not saved in an index. Like the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) constructors, the method raises `RuntimeError` for an index created by a previous version; such an index must be updated first (see [Update index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md#update-index-version" >}})). Below is an example of creating an on-disk index and converting it to an in-memory index.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings
from groupdocs.search.common import FileLogger

# Creating a regular index on disk
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Closing the regular index
index.dispose()

# Loading the index entirely in memory for increased performance
# The logger is not saved in the index, so it is passed with the settings
settings = IndexSettings()
settings.logger = FileLogger("./log.txt", 4.0)
in_memory_index = Index.load_into_memory_completely("./index", settings)

# Searching in the index
query = "focus"
result = in_memory_index.search(query)
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-location/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-location/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-location/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "log.txt" >}}  
```text
2026-10-29 19:05:05.063	Index loaded into memory completely.
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-location/search_index_location_2/log.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
