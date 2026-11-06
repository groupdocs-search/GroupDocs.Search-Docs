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

Since version 26.11, an error that stops the search, for example a query that cannot be parsed, gives an empty result without the token of the next chunk, so the search by chunks ends. The error is reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index and in the [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) property of the result (see the [Search results]({{< ref "search/net/developer-guide/advanced-usage/searching/search-results.md" >}}) page).

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

A chunk is a segment of the index. Since version 26.10, the chunks are determined as follows (before, a search by chunks returned a chunk for every segment of the index):

*   Only the index segments in which the query can find matches are searched; the other segments are skipped. For example, a word that occurs in 10 of 256 segments of an index gives 10 chunks instead of 256. A query whose terms are in no segment gives one empty chunk. A chunk can still be empty, for example when the words of an AND query are in different documents of its segment, or when the words of a phrase are not side by side. All chunks together contain the same documents as a search without chunks.
*   The number of chunks is not known in advance and can differ from search to search, also for the same query. Therefore, always iterate over the chunks while the [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/nextchunksearchtoken/) property is not null, and do not rely on the number of index segments.

The chunks of a search are taken from the segments that the index has when the search starts. The segments added to the index after the start of the search, for example by indexing performed at the same time, are not searched.

Searches and index-modifying operations do not block each other (see the [Search thread safety]({{< ref "search/net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}}) page). If the index is changed after the start of a search by chunks, for example, documents are added, updated or deleted, or the index is optimized, the [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) property of the result of a subsequent chunk contains the warning "Index has been changed since chunk search start, therefore results may be incorrect.". In this case, it is recommended to restart the search.

To avoid searching the index again for each chunk, the words found by wildcard, regular expression, numeric range and fuzzy search queries are kept between the chunks of a search and are reused by subsequent searches with the same query until the index is changed. Since version 26.11, a search by chunks also takes the words that a search without chunks has found for the same part of a query. For this, an index stored on disk can create temporary files in the index folder (normally up to 256 MB in total). These files are deleted automatically. An index in memory creates no such files.

## Search by chunks in an index repository

Since version 26.11, the search by chunks can also be performed in an [index repository]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-repository.md" >}}). The chunks of the indexes of the repository follow one another, index by index: the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/search/) method of the [IndexRepository](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/) class with the [IsChunkSearch](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/ischunksearch/) flag returns the first chunk of the first index, and the [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/searchnext/) method of the repository continues the search with the token of the previous result through the indexes that the repository has when the search starts. A document that several indexes of the repository contain is found in a chunk of each of them. The SearchNext method of the repository takes only a token of a search of the same repository; another token throws ArgumentException. Like the SearchNext method of an index, it has an overload that takes a [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/) object (see [Cancellation of search by chunks](#cancellation-of-search-by-chunks)). Before version 26.11, a search by chunks of a repository of several indexes returned the first chunks of all its indexes in one result without a token of the next chunk.

The following example demonstrates the search by chunks in an index repository.

**C#**

```csharp
string indexFolder1 = @"c:\MyIndex1\";
string indexFolder2 = @"c:\MyIndex2\";
string documentsFolder1 = @"c:\MyDocuments1\";
string documentsFolder2 = @"c:\MyDocuments2\";
string query = "Einstein";

// Creating an index repository with two indexes
IndexRepository repository = new IndexRepository();
Index index1 = repository.Create(indexFolder1);
index1.Add(documentsFolder1);
Index index2 = repository.Create(indexFolder2);
index2.Add(documentsFolder2);

// Creating a search options instance
SearchOptions options = new SearchOptions();
options.IsChunkSearch = true; // Enabling the search by chunks

// Starting the search by chunks in the indexes of the repository, one index after another
SearchResult result = repository.Search(query, options);
Console.WriteLine("Document count: " + result.DocumentCount);

// Continuing the search by chunks while there are chunks left
while (result.NextChunkSearchToken != null)
{
    result = repository.SearchNext(result.NextChunkSearchToken);
    Console.WriteLine("Document count: " + result.DocumentCount);
}
```

## Cancellation of search by chunks

The cancellation object specified in the [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/cancellation/) property of the search options applies to the first call to the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method. To be able to cancel the search of subsequent chunks, use the [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search/index/searchnext/) method overload that takes a [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/) object as the second argument. The result of a canceled chunk contains no documents, and its [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) property contains the message "Operation cancelled.". Since version 26.11, a cancellation object can be disposed of, for example by a using statement, to release the timer of the [CancelAfter](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/cancelafter/) method (see the [Search options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md" >}}) page).

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
// The using statement disposes of the cancellation object and releases its timer
using (Cancellation cancellation = new Cancellation())
{
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
