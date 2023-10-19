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
All search network shards can be optimized with a single call the [Optimize](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/optimize/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class.

The optimization minimizes the number of index segments on each shard. As a result, search performance increases.

An asynchronous execution flag is available as an option. In the case of asynchronous optimization, you can track the completion of the operation by subscribing to the [OptimizationCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/optimizationcompleted/) event.

The following code example demonstrates how to perform search network optimization.

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
