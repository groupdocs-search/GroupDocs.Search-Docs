---
id: optimizing-shards
url: search/net/optimizing-shards
title: Optimizing shards
weight: 60
description: "This page contains information about optimizing shards in the search network."
keywords: optimize search network, optimize distributed index, search network optimization, distributed index optimization, shard optimization
productName: GroupDocs.Search for .NET
hideChildren: False
---
All search network shards can be optimized with a single call of the [Optimize](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/optimize/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class.

The optimization merges the index segments of each shard, minimizing their number, and physically removes the data of deleted documents. As a result, search performance increases and the shards take less disk space. After optimizing a very large index, a shard may still contain several segments. The progress of the optimization can be tracked by subscribing to the [OptimizationProgressChanged](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/optimizationprogresschanged/) event.

An asynchronous execution flag is available as an option ([OptimizeOptions.IsAsync](https://reference.groupdocs.com/search/net/groupdocs.search.options/optimizeoptions/isasync/)). In the case of asynchronous optimization, you can track the completion of the operation by subscribing to the [OptimizationCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/optimizationcompleted/) event.

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

The following code example, which continues the previous one, demonstrates how to perform search network optimization.

**C#**

```csharp
Console.WriteLine("Optimizing shards");
Indexer indexer = node.Indexer;
OptimizeOptions options = new OptimizeOptions();
indexer.Optimize(options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
