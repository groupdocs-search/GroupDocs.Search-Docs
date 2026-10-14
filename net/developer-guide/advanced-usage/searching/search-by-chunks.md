---
id: search-by-chunks
url: search/net/search-by-chunks
title: Search by chunks
weight: 18
description: "This article describes search by chunks, which returns the results of a search in parts."
keywords: Search by chunks
productName: GroupDocs.Search for .NET
hideChildren: False
---
The GroupDocs.Search API provides the ability to perform search by chunks. In this mode, a search returns its results in parts (chunks): each call to the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) or [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search/index/searchnext/) method of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class searches only a part of the index and returns the documents found in this part. This feature becomes relevant when searching in large indexes containing tens and hundreds of thousands of documents, because the first results are available before the whole index is searched.

## How to perform search by chunks

When performing search by chunks, the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method is first called with the [IsChunkSearch](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/ischunksearch/) flag set to true in the search options. The search result contains the documents found in the first chunk. Then each subsequent chunk is searched by calling the [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search/index/searchnext/) method with the [ChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.common/chunksearchtoken/) object returned by the [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/nextchunksearchtoken/) property of the previous search result. The token contains the search query and the search options, so the same query is searched in each chunk. The search is complete when the [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/nextchunksearchtoken/) property returns null.

The following example demonstrates the search by chunks.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string query = "Einstein";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Creating a search options instance
SearchOptions options = new SearchOptions();
options.IsChunkSearch = true; // Enabling the search by chunks

// Starting the search by chunks
SearchResult result = index.Search(query, options);
Console.WriteLine("Document count: " + result.DocumentCount);
Console.WriteLine("Occurrence count: " + result.OccurrenceCount);

// Continuing the search by chunks while there are chunks left
while (result.NextChunkSearchToken != null)
{
    result = index.SearchNext(result.NextChunkSearchToken);
    Console.WriteLine("Document count: " + result.DocumentCount);
    Console.WriteLine("Occurrence count: " + result.OccurrenceCount);
}
```

## Chunks of a search

Each chunk corresponds to a segment of the index. Since version 26.10, the chunks are determined as follows:

*   Only the index segments in which the query can find matches are searched; the other segments are skipped. For example, a word that occurs in 10 of 256 segments of an index gives 10 chunks instead of 256.
*   The number of chunks is not known in advance and can differ from search to search. Therefore, always iterate over the chunks while the [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/nextchunksearchtoken/) property is not null, and do not rely on the number of index segments.
*   The chunks of a search are taken from the segments that the index has when the search starts. The segments added to the index after the start of the search, for example by indexing performed at the same time, are not searched.

Searches and index-modifying operations do not block each other (see the [Search thread safety]({{< ref "search/net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}}) page). If the index is changed after the start of a search by chunks, for example, documents are added, updated or deleted, or the index is optimized, the [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) property of the result of a subsequent chunk contains the warning "Index has been changed since chunk search start, therefore results may be incorrect.". In this case, it is recommended to restart the search.

To avoid searching the index again for each chunk, the words found by wildcard, regular expression, numeric range and fuzzy search queries are kept between the chunks of a search and are reused by subsequent searches with the same query. For this, an index stored on disk can create temporary files in the index folder (normally up to 256 MB in total). These files are deleted automatically.

## Cancellation of search by chunks

The cancellation object specified in the [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/cancellation/) property of the search options applies to the first call to the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method. To be able to cancel the search of subsequent chunks, use the [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search/index/searchnext/) method overload that takes a [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/) object as the second argument. The result of a canceled chunk contains no documents, and its [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) property contains the message "Operation cancelled.".

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string query = "Einstein";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Creating a cancellation object that cancels the search after 10 seconds
Cancellation cancellation = new Cancellation();
cancellation.CancelAfter(10000);

// Creating a search options instance
SearchOptions options = new SearchOptions();
options.IsChunkSearch = true; // Enabling the search by chunks
options.Cancellation = cancellation; // Cancellation of the first chunk

// Starting the search by chunks
SearchResult result = index.Search(query, options);
Console.WriteLine("Document count: " + result.DocumentCount);

// Continuing the search by chunks until the search is complete or canceled
while (result.NextChunkSearchToken != null && !cancellation.IsCancelled)
{
    result = index.SearchNext(result.NextChunkSearchToken, cancellation);
    Console.WriteLine("Document count: " + result.DocumentCount);
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
