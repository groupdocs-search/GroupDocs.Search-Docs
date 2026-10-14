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

To continue the search, the [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/searchnext/) method is used, returning the second and subsequent portions of results. Continue the search while the [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/nextchunksearchtoken/) property of the result is not *null*.

Text and object forms of the search query are supported when appropriate overloads of the SearchFirst method are used. The options for a network search are the same as those for a single index search.

Each portion of results is returned as a [NetworkSearchResult](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/) object, which has the following members:

 - [DocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/documentcount/) and [OccurrenceCount](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/occurrencecount/) — the number of found documents and the total number of occurrences found;
 - [GetFoundDocument](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/getfounddocument/) — returns a found document by its index; the result can also be enumerated;
 - [Truncated](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/truncated/) and [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/warnings/) — indicate whether the result is truncated and describe the result;
 - [StartTime](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/starttime/), [EndTime](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/endtime/) and [SearchDuration](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/searchduration/) — the time of the search;
 - [NodeIndex](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/nodeindex/) — the index of the node from which the result was received;
 - [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networksearchresult/nextchunksearchtoken/) — the token for continuing the search, or *null* if the search is complete.

A found document is represented by the [NetworkFoundDocument](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networkfounddocument/) class, which contains the [FoundDocument](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networkfounddocument/founddocument/) object, as in a single index search, and the index of the shard where the document was found ([ShardIndex](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networkfounddocument/shardindex/) property).

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

The following code example, which continues the previous one, demonstrates performing a full-text search on the search network.

**C#**

```csharp
string word = "einstein";
bool useSynonymSearch = true;

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

documents.AddRange(result);
totalOccurrences += result.OccurrenceCount;
Console.WriteLine("Documents found (node " + result.NodeIndex + "): " + result.DocumentCount);

while (result.NextChunkSearchToken != null)
{
    Console.WriteLine();
    Console.WriteLine("Next time search for: " + query);

    result = searcher.SearchNext(result.NextChunkSearchToken);

    documents.AddRange(result);
    totalOccurrences += result.OccurrenceCount;
    Console.WriteLine("Documents found (node " + result.NodeIndex + "): " + result.DocumentCount);
}

Console.WriteLine();
Console.WriteLine("Total occurrences: " + totalOccurrences);
for (int i = 0; i < documents.Count; i++)
{
    NetworkFoundDocument document = documents[i];
    Console.WriteLine(document.ShardIndex + ": " + document.FoundDocument.DocumentInfo.FilePath +
        " (" + document.FoundDocument.OccurrenceCount + " occurrences)");
}
```

More information about search queries can be found on the [Queries in text and object form]({{< ref "search/net/developer-guide/advanced-usage/searching/queries-in-text-and-object-form.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
