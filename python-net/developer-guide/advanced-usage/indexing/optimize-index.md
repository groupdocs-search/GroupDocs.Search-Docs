---
id: optimize-index
url: search/python-net/optimize-index
title: Optimize index
weight: 22
description: "This article explains how to optimize an index to reduce the number of its segments and physically remove deleted documents."
keywords: optimize index, index segments, merge segments, remove deleted documents, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
An index optimization operation allows you to reduce the number of segments in an index, thereby increasing search performance in the index. Each indexing operation adds at least one new segment to an index, so an index to which documents are often added should be optimized from time to time.

The optimization also physically removes [deleted documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/delete-indexed-documents.md" >}}) from the index. Deleting documents does not rewrite the index: deleted documents are excluded from search immediately, but their data remain in the index files until the index is optimized.

The memory consumption of the optimization does not depend on the index size: a very large index is optimized into several segments of limited size, so the [segment_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexinfo/segment_count/) property of [Index.index_info](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/index_info/) may be greater than 1 after optimizing such an index.

To perform this operation, call the [optimize](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/optimize/) method without arguments or with the [MergeOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/mergeoptions/) instance as an argument. In the [MergeOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/mergeoptions/) class, there is a property for specifying a cancellation object and a property for specifying whether to perform the operation asynchronously. By default, the operation is performed synchronously.

The following example demonstrates how to perform the index optimization.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.common import Cancellation
from groupdocs.search.options import MergeOptions

# Creating an index in the specified folder
index = Index("./index")

index.add("./documents")  # Indexing documents from the specified folder
index.add("./documents2")  # Each call to add creates at least one new segment in the index
index.add("./documents3")

options = MergeOptions()
options.cancellation = Cancellation()  # Creating cancellation object to be able to cancel the operation
options.cancellation.cancel_after(100000)  # Setting maximum duration of the operation to 100 seconds

# Merging segments of the index
index.optimize(options)

print("Segment count: " + str(index.index_info.segment_count))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/optimize-index/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/optimize-index/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "documents2" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents2/Curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/optimize-index/documents2/Curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "documents3" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents3/Bohr.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/optimize-index/documents3/Bohr.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "optimize-index.txt" >}}  
```text
Segment count: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/optimize-index/optimize_index/optimize-index.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
