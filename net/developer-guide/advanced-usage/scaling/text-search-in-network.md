---
id: text-search-in-network
url: search/net/text-search-in-network
title: Text search in network
weight: 90
description: "This page contains information about full-text searching in the search network."
keywords: full-text search in network, text search in distributed index
productName: GroupDocs.Search for .NET
hideChildren: False
---
To run a text search, use the [SearchFirst](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/searchfirst/#searchfirst_1) method of the [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/) class. This method returns the first portion of results.

To continue the search, the [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/searchnext/) method is used, returning the second and subsequent portions of results.

Currently, only the object form of the search query is supported. The options for a network search are the same as those for a single index search.

The following code example demonstrates performing a full-text search on the search network.

**C#**

```csharp
Searcher searcher = node.Searcher;

SearchQuery query = SearchQuery.CreateWordQuery(word);
Console.WriteLine();
Console.WriteLine("First time search for: " + query);
SearchOptions options = new SearchOptions();
options.IsChunkSearch = true;
options.UseSynonymSearch = useSynonymSearch;
int totalOccurrences = 0;
List<NetworkFoundDocument> documents = new List<NetworkFoundDocument>();

NetworkSearchResult result = searcher.SearchFirst(query, options);

AddDocsFromResult(documents, result);
totalOccurrences += result.OccurrenceCount;
TraceResult(result);

while (result.NextChunkSearchToken != null)
{
    Console.WriteLine();
    Console.WriteLine("Next time search for: " + query);

    result = searcher.SearchNext(result.NextChunkSearchToken);

    AddDocsFromResult(documents, result);
    totalOccurrences += result.OccurrenceCount;
    TraceResult(result);
}

Console.WriteLine();
Console.WriteLine("Total occurrences: " + totalOccurrences);
```

More information about search queries can be found in the article [Queries in text and object form](https://docs.groupdocs.com/search/net/queries-in-text-and-object-form/).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
