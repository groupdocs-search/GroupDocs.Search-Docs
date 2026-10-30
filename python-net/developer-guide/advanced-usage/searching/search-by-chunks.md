---
id: search-by-chunks
url: search/python-net/search-by-chunks
title: Search by chunks
weight: 18
description: "This article describes search by chunks, which returns the results of a search in parts."
keywords: Search by chunks
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The GroupDocs.Search API provides the ability to perform search by chunks. In this mode, a search returns its results in parts (chunks): each call to the [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search/) or [search_next](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search_next/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class searches only a part of the index and returns the documents found in this part. This feature becomes relevant when searching in large indexes containing tens and hundreds of thousands of documents, because the first results are available before the whole index is searched.

## How to perform search by chunks

When performing search by chunks, the [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search/) method is first called with the [is_chunk_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/is_chunk_search/) flag set to True in the search options. The search result contains the documents found in the first chunk. Then each subsequent chunk is searched by calling the [search_next](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search_next/) method with the [ChunkSearchToken](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/chunksearchtoken/) object returned by the [next_chunk_search_token](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/next_chunk_search_token/) property of the previous search result. The token contains the search query and the search options, so the same query is searched in each chunk. The search is complete when the [next_chunk_search_token](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/next_chunk_search_token/) property returns None.

The following example demonstrates the search by chunks. To produce several chunks with a few small documents, the example indexes the documents one by one: each call to the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/add/) method creates a new index segment.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchOptions

query = "Einstein"

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder one by one, so that each document gets its own index segment
for file_name in sorted(os.listdir("./documents")):
    index.add(os.path.join("./documents", file_name))
print("Index segments: " + str(index.index_info.segment_count))

# Creating a search options instance
options = SearchOptions()
options.is_chunk_search = True  # Enabling the search by chunks

# Starting the search by chunks
result = index.search(query, options)
print("Document count: " + str(result.document_count))
print("Occurrence count: " + str(result.occurrence_count))

# Continuing the search by chunks while there are chunks left
while result.next_chunk_search_token is not None:
    result = index.search_next(result.next_chunk_search_token)
    print("Document count: " + str(result.document_count))
    print("Occurrence count: " + str(result.occurrence_count))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/gravity-notes.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "how-to-perform-search-by-chunks.txt" >}}  
```text
Index segments: 5
Document count: 1
Occurrence count: 3
Document count: 1
Occurrence count: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/search-by-chunks/how_to_perform_search_by_chunks/how-to-perform-search-by-chunks.txt)
{{< /tab >}}
{{< /tabs >}}

## Chunks of a search

Each chunk corresponds to a segment of the index. Since version 26.10, the chunks are determined as follows:

*   Only the index segments in which the query can find matches are searched; the other segments are skipped. For example, a word that occurs in 10 of 256 segments of an index gives 10 chunks instead of 256.
*   The number of chunks is not known in advance and can differ from search to search. Therefore, always iterate over the chunks while the [next_chunk_search_token](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/next_chunk_search_token/) property is not None, and do not rely on the number of index segments.
*   The chunks of a search are taken from the segments that the index has when the search starts. The segments added to the index after the start of the search, for example by indexing performed at the same time, are not searched.

Searches and index-modifying operations do not block each other (see the [Search thread safety]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}}) page). If the index is changed after the start of a search by chunks, for example, documents are added, updated or deleted, or the index is optimized, the [warnings](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/warnings/) property of the result of a subsequent chunk contains the warning "Index has been changed since chunk search start, therefore results may be incorrect.". In this case, it is recommended to restart the search.

To avoid searching the index again for each chunk, the words found by wildcard, regular expression, numeric range and fuzzy search queries are kept between the chunks of a search and are reused by subsequent searches with the same query. For this, an index stored on disk can create temporary files in the index folder (normally up to 256 MB in total). These files are deleted automatically: when the index is disposed, for example, at the end of a `with Index(...) as index:` block, or when the index is opened again.

## Cancellation of search by chunks

The cancellation object specified in the [cancellation](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/cancellation/) property of the search options applies to the first call to the [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search/) method. To be able to cancel the search of subsequent chunks, use the [search_next](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search_next/) method overload that takes a [Cancellation](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/cancellation/) object as the second argument. The result of a canceled chunk contains no documents, and its [warnings](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/warnings/) property contains the message "Operation cancelled.".

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import Cancellation
from groupdocs.search.options import SearchOptions

query = "Einstein"

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder one by one, so that each document gets its own index segment
for file_name in sorted(os.listdir("./documents")):
    index.add(os.path.join("./documents", file_name))

# Creating a cancellation object that cancels the search after 10 seconds
cancellation = Cancellation()
cancellation.cancel_after(10000)

# Creating a search options instance
options = SearchOptions()
options.is_chunk_search = True  # Enabling the search by chunks
options.cancellation = cancellation  # Cancellation of the first chunk

# Starting the search by chunks
result = index.search(query, options)
print("Document count: " + str(result.document_count))

# Continuing the search by chunks until the search is complete or canceled
while result.next_chunk_search_token is not None and not cancellation.is_cancelled:
    result = index.search_next(result.next_chunk_search_token, cancellation)
    print("Document count: " + str(result.document_count))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/gravity-notes.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-by-chunks/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "cancellation-of-search-by-chunks.txt" >}}  
```text
Document count: 1
Document count: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/search-by-chunks/cancellation_of_search_by_chunks/cancellation-of-search-by-chunks.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
