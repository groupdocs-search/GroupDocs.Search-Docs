---
id: image-search-in-network
url: search/net/image-search-in-network
title: Image search in network
weight: 100
description: "This page contains information about reverse image search in the search network."
keywords: image search in network, image search in distributed index, reverse image search in network
productName: GroupDocs.Search for .NET
hideChildren: False
---
To run a reverse image search, use the [SearchFirst](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/searchfirst/#searchfirst) method of the [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/) class. This method returns the first portion of results.

To continue the search, the [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/searchnext/#searchnext) method is used, returning the second and subsequent portions of results.

The search image object and search options object are the same as those used for single index searches.

The following code example demonstrates performing a reverse image search on the search network.

**C#**

```csharp
Console.WriteLine("First image search");

Searcher searcher = node.Searcher;
ImageSearchOptions options = new ImageSearchOptions();
options.HashDifferences = hashDifferences;
int total = 0;

NetworkImageSearchResult result = searcher.SearchFirst(searchImage, options);
Console.WriteLine("Images found (shard " + result.ShardIndex + "): " + result.ImageCount);
total += result.ImageCount;

while (result.NetworkImageSearchToken != null)
{
    Console.WriteLine();
    Console.WriteLine("Next image search");

    result = searcher.SearchNext(result.NetworkImageSearchToken);
    Console.WriteLine("Images found (shard " + result.ShardIndex + "): " + result.ImageCount);
    total += result.ImageCount;
}

Console.WriteLine();
Console.WriteLine("Total images found (diffs = " + hashDifferences + "): " + total);
```

More information about reverse image search in the [article](https://docs.groupdocs.com/search/net/reverse-image-search/).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
