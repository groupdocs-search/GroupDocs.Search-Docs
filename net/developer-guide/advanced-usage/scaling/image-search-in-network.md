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
To run a reverse image search, use the [SearchFirst](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/searchfirst/#searchfirst) method of the [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/) class. This method returns the first portion of results as a [NetworkImageSearchResult](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networkimagesearchresult/) object, which contains the images found in one shard.

To continue the search, the [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/searchnext/#searchnext) method is used, returning the second and subsequent portions of results. Continue the search while the [NetworkImageSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networkimagesearchresult/networkimagesearchtoken/) property of the result is not *null*.

The search image object and search options object are the same as those used for single index searches. Images can be found only if image indexing was enabled when the documents were [added]({{< ref "search/net/developer-guide/advanced-usage/scaling/indexing-documents.md" >}}).

The examples on this page use the master node of a running search network. The following code creates and starts the master node as described on the [Search network deployment]({{< ref "search/net/developer-guide/advanced-usage/scaling/search-network-deployment.md" >}}) page; the slave nodes must be started beforehand.

**C#**

```csharp
string basePath = @"c:\SearchNetwork\";
int basePort = 49100;

// Loading the search network configuration from a file
Configuration configuration = Configuration.Load(basePath + "Configuration.xml");

// Creating, configuring and starting the master node; the timeouts are specified in milliseconds
SearchNetworkNode node = SearchNetworkNode.CreateMasterNode(
    0,
    basePath + "Node0",
    new TcpSettings(basePort, 3000, 3000, 3000, 1000),
    new FileLogger(basePath + "Log.txt", 4.0),
    configuration);
node.ConfigureAllNodes();
node.Start();
```

The following code example, which continues the previous one, demonstrates performing a reverse image search on the search network.

**C#**

```csharp
// Creating a search image from a file
SearchImage searchImage = SearchImage.Create(@"c:\MyDocuments\Image.png");
int hashDifferences = 8; // The maximum allowed difference between the image hashes

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

More information about reverse image search can be found on the [Reverse image search]({{< ref "search/net/developer-guide/advanced-usage/searching/reverse-image-search.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
