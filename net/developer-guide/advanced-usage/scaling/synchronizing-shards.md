---
id: synchronizing-shards
url: search/net/synchronizing-shards
title: Synchronizing shards
weight: 70
description: "This page contains information about synchronizing shards in the search network."
keywords: synchronize search network, synchronize distributed index, search network synchronization, distributed index synchronization, shard synchronization
productName: GroupDocs.Search for .NET
hideChildren: False
---
If there is any communication interruption in the search network, some operations may not complete correctly. To resolve such problems, call the [Synchronize](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/synchronize/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class. The method synchronizes the list of indexed documents kept by the indexer with the documents in the shards.

The asynchronous operation flag is available as an option ([SynchronizeOptions.IsAsync](https://reference.groupdocs.com/search/net/groupdocs.search.options/synchronizeoptions/isasync/)). You can track the completion of the operation by subscribing to the [SynchronizationCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/synchronizationcompleted/) event.

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

The following code example, which continues the previous one, demonstrates how to synchronize search network nodes.

**C#**

```csharp
Console.WriteLine("Synchronizing shards");
Indexer indexer = node.Indexer;
SynchronizeOptions options = new SynchronizeOptions();
indexer.Synchronize(options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
