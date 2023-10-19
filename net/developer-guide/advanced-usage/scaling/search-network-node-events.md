---
id: search-network-node-events
url: search/net/search-network-node-events
title: Search network node events
weight: 30
description: "This page contains information about the purpose and use of all search network events."
keywords: search network events, network events, subscribe to event, track progress
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains information about the purpose and use of all search network events.

## ConfigurationCompleted event

The [ConfigurationCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/configurationcompleted/) event occurs when the search network configuration process is finished.

## IndexingCompleted event

The [IndexingCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/indexingcompleted/) event occurs when indexing of all enqueued documents is finished.

## DeletionCompleted event

The [DeletionCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/deletioncompleted/) event occurs when all enqueued deletions of documents are finished.

## OptimizationCompleted event

The [OptimizationCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/optimizationcompleted/) event occurs when optimization of all nodes is finished.

## SynchronizationCompleted event

The [SynchronizationCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/synchronizationcompleted/) event occurs when synchronization with all nodes is finished.

## AttributeChangesCompleted event

The [AttributeChangesCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/attributechangescompleted/) event occurs when all enqueued attribute changes are finished.

## StatusChanged event

The [StatusChanged](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/statuschanged/) event occurs when the search network status changes.

## DataExtracted event

The [DataExtracted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/dataextracted/) event occurs when the data has been extracted from a document.

## DocumentIndexed event

The [DocumentIndexed](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/documentindexed/) event occurs when a document has been indexed.

## DocumentDeleted event

The [DocumentDeleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/documentdeleted/) event occurs when a document has been deleted.

## ErrorOccurred event

The [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/erroroccurred/) event occurs when an error occurs in one of the nodes of the search network.

## IndexingProgressChanged event

The [IndexingProgressChanged](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/indexingprogresschanged/) event occurs when the progress of the indexing operation has changed.

## OptimizationProgressChanged event

The [OptimizationProgressChanged](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/optimizationprogresschanged/) event occurs when the progress of the optimization operation has changed.

## Code example

**C#**

```csharp
node.Events.IndexingCompleted += (s, e) =>
{
    Console.WriteLine("Indexing complete");
};
node.Events.DeletionCompleted += (s, e) =>
{
    Console.WriteLine("Deleting complete");
};
node.Events.OptimizationCompleted += (s, e) =>
{
    Console.WriteLine("Optimization complete");
};
node.Events.SynchronizationCompleted += (s, e) =>
{
    Console.WriteLine("Synchronization complete");
};
node.Events.AttributeChangesCompleted += (s, e) =>
{
    Console.WriteLine("Attribute changes complete");
};
node.Events.StatusChanged += (s, e) =>
{
    Console.WriteLine("Status changed: " + e.OldStatus + " -> " + e.NewStatus);
};
node.Events.DataExtracted += (s, e) =>
{
    Console.WriteLine("Data extracted (" + e.ExtractorIndex + "): " + e.DocumentKey);
};
node.Events.DocumentIndexed += (s, e) =>
{
    Console.WriteLine("Document indexed (" + e.ShardIndex + "): " + e.DocumentKey);
};
node.Events.DocumentDeleted += (s, e) =>
{
    Console.WriteLine("Document deleted (" + e.ShardIndex + "): " + e.DocumentKey);
};
node.Events.ErrorOccurred += (s, e) =>
{
    Console.WriteLine("Error occurred (" + e.NodeIndex + ", " + e.ServiceIndex + "): " + e.Message);
};
node.Events.IndexingProgressChanged += (s, e) =>
{
    Console.WriteLine("Indexing progress changed (" + e.NodeIndex + ", " + e.ServiceIndex + "): " + e.ProgressPercentage);
};
node.Events.OptimizationProgressChanged += (s, e) =>
{
    Console.WriteLine("Optimization progress changed (" + e.NodeIndex + ", " + e.ServiceIndex + "): " + e.ProgressPercentage);
};
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
