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
If there is any communication interruption in the search network, some operations may not complete correctly. To resolve such problems, you need to call the [Synchronize](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/synchronize/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class.

The asynchronous operation flag is available as an option. You can track the completion of the operation by subscribing to the [SynchronizationCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/synchronizationcompleted/) event.

The following code example demonstrates how to synchronize search network nodes.

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
