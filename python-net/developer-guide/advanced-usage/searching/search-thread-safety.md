---
id: search-thread-safety
url: search/python-net/search-thread-safety
title: Search thread safety
weight: 25
description: "This article describes the thread safety of search operations and how searches interact with index-modifying operations."
keywords: Search thread safety
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
## Multiple simultaneous searches

Search in an index is a thread safe operation. This means that calls to the [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search/) and [search_next](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search_next/) methods can be made from different threads to the same instance of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class without any cross-threading problems.

## Search during index-modifying operations

Since version 26.10, searches and index-modifying operations do not block each other. A search can be performed from any thread even while documents are being indexed, updated or deleted, or while the index is being merged or optimized. Each search works with a consistent snapshot of the index taken when the search starts: the changes made by an operation that is performed at the same time do not affect the search that has already started. For the specifics of search by chunks, see the [Search by chunks]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) page.

## Index-modifying operations

Index-modifying operations cannot be performed at the same time. The following operations are mutually exclusive:

*   indexing documents ([add](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/add/));
*   updating the index ([update](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/update/));
*   deleting documents from the index ([delete](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/delete/));
*   merging indexes ([merge](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/merge/));
*   optimizing the index ([optimize](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/optimize/));
*   changing document attributes ([change_attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/change_attributes/));
*   checking index segments ([check_segments](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/check_segments/));
*   saving the index to another folder ([save_to](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/save_to/));
*   renaming an indexed document ([notify](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/notify/) with a rename notification).

The current state of the index is available in the [IndexInfo.index_status](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexinfo/index_status/) property. If one of these operations is started before the previous one has finished, for example from another thread or while an asynchronous indexing operation is in progress, the new call does not throw an exception and does nothing: the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index is raised with the message "A new operation can not start until the previous one has finished" (for a merge or an optimization, the message is "Merged indexes must not be in progress state."). The [notify](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/notify/) method returns False in this case.

Details on performing these operations can be found in the [Indexing]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/_index.md" >}}) section and on the [Update index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md" >}}), [Delete indexed documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/delete-indexed-documents.md" >}}), [Merge indexes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/merge-indexes.md" >}}), [Optimize index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}), [Document attributes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}), [Document renaming]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-renaming.md" >}}) and [Search index location]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-location.md" >}}) pages.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
