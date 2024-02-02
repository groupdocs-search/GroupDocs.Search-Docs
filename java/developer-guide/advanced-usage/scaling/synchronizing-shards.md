---
id: synchronizing-shards
url: search/java/synchronizing-shards
title: Synchronizing shards
weight: 70
description: "This page contains information about synchronizing shards in the search network."
keywords: synchronize search network, synchronize distributed index, search network synchronization, distributed index synchronization, shard synchronization
productName: GroupDocs.Search for Java
hideChildren: False
---
If there is any communication interruption in the search network, some operations may not complete correctly. To resolve such problems, you need to call the [synchronize](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/#synchronize-com.groupdocs.search.options.SynchronizeOptions-) method of the [Indexer](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/) class.

The asynchronous operation flag is available as an option. You can track the completion of the operation by subscribing to the [SynchronizationCompleted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#SynchronizationCompleted) event.

The following code example demonstrates how to synchronize search network nodes.

```java
System.out.println("Synchronizing shards");
Indexer indexer = node.getIndexer();
SynchronizeOptions options = new SynchronizeOptions();
indexer.synchronize(options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
