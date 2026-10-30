---
id: merge-indexes
url: search/python-net/merge-indexes
title: Merge indexes
weight: 20
description: "This article demonstrates the merge operation which is designed to combine two or more indexes into one index to accelerate the search and to simplify the work with indexes."
keywords: merge indexes, merge operation, index repository, MergeOptions, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The merge operation is designed to combine two or more indexes into one index to accelerate the search and to simplify the work with indexes. When merging, only the index at which the [merge](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/merge/) method was called is changed. This index as a result of the operation contains all the documents that were contained in all indexes together. The second index or index repository after the merge can be deleted to free up disk space.

The [MergeOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/mergeoptions/) class contains a property for specifying a cancellation object, as well as a property for specifying whether the operation will be performed asynchronously. By default, index merging operation is performed synchronously.

The [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class merges either a single index or a whole index repository into the current index: pass an `Index` or an `IndexRepository` object to the [merge](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/merge/) method. Before performing a merge, checks are made to ensure that the indexes are of the same type. Merging is performed only on those indexes that have the same [index_type](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/index_type/) value as the index on which the [merge](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/merge/) method is called.

## Merging compatibility

Since version 26.10, only indexes that divide the text of documents into terms in the same way can be merged:

*   Before making any changes, the [merge](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/merge/) method compares the current index with every merged index that contains documents: the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/), [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/), [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) settings, the alphabet, the tokenization dictionary, and also the stop words and the character replacements if the indexes use them. If a difference is found, the method raises `RuntimeError`, whose message names the differences, and no index is changed. The exception's `message` attribute holds the message alone; `str(exception)` also contains the .NET exception type and stack trace. This also happens in the asynchronous mode.
*   The alphabet, the tokenization dictionary, the stop word dictionary and the character replacement dictionary of the current index are not changed by merging. The other dictionaries of the merged indexes (for example, synonyms and homophones) are merged into the dictionaries of the current index.
*   A merged index created by a previous version must be updated with the [IndexUpdater](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/) class before merging (see [Update index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md#update-index-version" >}})).
*   A merge is not performed if a merged index is performing an operation or has failed; the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event reports it.

Note that an index updated from a previous version has the [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) settings disabled, while a new index has them enabled by default. The full rules and an example of merging an updated index are presented on the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

## Merging example

The following example demonstrates the merging of two indexes.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.common import Cancellation
from groupdocs.search.options import MergeOptions

index1 = Index("./index1")  # Creating index1
index1.add("./documents")  # Indexing documents

index2 = Index("./index2")  # Creating index2
index2.add("./documents2")  # Indexing documents

options = MergeOptions()
options.cancellation = Cancellation()  # Creating cancellation object to be able to cancel the operation

try:
    # Merging index2 into index1
    # Files of index2 will not be changed
    index1.merge(index2, options)
except RuntimeError as exception:
    # The indexes divide text into terms differently; the message names the differences
    print(exception.message)

print("Documents in index1:", len(index1.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/merge-indexes/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/merge-indexes/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "documents2" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents2/Curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/merge-indexes/documents2/Curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "merging.txt" >}}  
```text
Documents in index1: 3
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/merge-indexes/merging_example/merging.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
