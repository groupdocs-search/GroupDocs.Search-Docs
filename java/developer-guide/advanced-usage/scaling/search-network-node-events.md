---
id: search-network-node-events
url: search/java/search-network-node-events
title: Search network node events
weight: 30
description: "This page contains information about the purpose and use of all search network events."
keywords: search network events, network events, subscribe to event, track progress
productName: GroupDocs.Search for Java
hideChildren: False
---
This page contains information about the purpose and use of all search network events.

## ConfigurationCompleted event

The [ConfigurationCompleted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#ConfigurationCompleted) event occurs when the search network configuration process is finished.

## IndexingCompleted event

The [IndexingCompleted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#IndexingCompleted) event occurs when indexing of all enqueued documents is finished.

## DeletionCompleted event

The [DeletionCompleted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#DeletionCompleted) event occurs when all enqueued deletions of documents are finished.

## OptimizationCompleted event

The [OptimizationCompleted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#OptimizationCompleted) event occurs when optimization of all nodes is finished.

## SynchronizationCompleted event

The [SynchronizationCompleted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#SynchronizationCompleted) event occurs when synchronization with all nodes is finished.

## AttributeChangesCompleted event

The [AttributeChangesCompleted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#AttributeChangesCompleted) event occurs when all enqueued attribute changes are finished.

## StatusChanged event

The [StatusChanged](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#StatusChanged) event occurs when the search network status changes.

## DataExtracted event

The [DataExtracted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#DataExtracted) event occurs when the data has been extracted from a document.

## DocumentIndexed event

The [DocumentIndexed](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#DocumentIndexed) event occurs when a document has been indexed.

## DocumentDeleted event

The [DocumentDeleted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#DocumentDeleted) event occurs when a document has been deleted.

## ErrorOccurred event

The [ErrorOccurred](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#ErrorOccurred) event occurs when an error occurs in one of the nodes of the search network.

## IndexingProgressChanged event

The [IndexingProgressChanged](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#IndexingProgressChanged) event occurs when the progress of the indexing operation has changed.

## OptimizationProgressChanged event

The [OptimizationProgressChanged](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#OptimizationProgressChanged) event occurs when the progress of the optimization operation has changed.

## Code example

```java
node.getEvents().IndexingCompleted.add(new EventHandler() {
    @Override
    public void invoke(Object s, EventArgs e) {
        System.out.println("Indexing complete");
    }
});
node.getEvents().DeletionCompleted.add(new EventHandler() {
    @Override
    public void invoke(Object s, EventArgs e) {
        System.out.println("Deleting complete");
    }
});
node.getEvents().OptimizationCompleted.add(new EventHandler() {
    @Override
    public void invoke(Object s, EventArgs e) {
        System.out.println("Optimization complete");
    }
});
node.getEvents().SynchronizationCompleted.add(new EventHandler() {
    @Override
    public void invoke(Object s, EventArgs e) {
        System.out.println("Synchronization complete");
    }
});
node.getEvents().AttributeChangesCompleted.add(new EventHandler() {
    @Override
    public void invoke(Object s, EventArgs e) {
        System.out.println("Attribute changes complete");
    }
});
node.getEvents().StatusChanged.add(new EventHandler<StatusChangedEventArgs>() {
    @Override
    public void invoke(Object s, StatusChangedEventArgs e) {
        System.out.println("Status changed: " + e.getOldStatus().name() + " -> " + e.getNewStatus().name());
    }
});
node.getEvents().DataExtracted.add(new EventHandler<DataExtractedEventArgs>() {
    @Override
    public void invoke(Object s, DataExtractedEventArgs e) {
        System.out.println("Data extracted (" + e.getExtractorIndex() + "): " + e.getDocumentKey());
    }
});
node.getEvents().DocumentIndexed.add(new EventHandler<DocumentIndexedEventArgs>() {
    @Override
    public void invoke(Object s, DocumentIndexedEventArgs e) {
        System.out.println("Document indexed (" + e.getShardIndex() + "): " + e.getDocumentKey());
    }
});
node.getEvents().DocumentDeleted.add(new EventHandler<DocumentDeletedEventArgs>() {
    @Override
    public void invoke(Object s, DocumentDeletedEventArgs e) {
        System.out.println("Document deleted (" + e.getShardIndex() + "): " + e.getDocumentKey());
    }
});
node.getEvents().ErrorOccurred.add(new EventHandler<ErrorOccurredEventArgs>() {
    @Override
    public void invoke(Object s, ErrorOccurredEventArgs e) {
        System.out.println("Error occurred (" + e.getNodeIndex() + ", " + e.getServiceIndex() + "): " + e.getMessage());
    }
});
node.getEvents().IndexingProgressChanged.add(new EventHandler<NetworkIndexingProgressEventArgs>() {
    @Override
    public void invoke(Object s, NetworkIndexingProgressEventArgs e) {
        System.out.println("Indexing progress changed (" + e.getNodeIndex() + ", " + e.getServiceIndex() + "): " + e.getProgressPercentage());
    }
});
node.getEvents().OptimizationProgressChanged.add(new EventHandler<NetworkOptimizationProgressEventArgs>() {
    @Override
    public void invoke(Object s, NetworkOptimizationProgressEventArgs e) {
        System.out.println("Optimization progress changed (" + e.getNodeIndex() + ", " + e.getServiceIndex() + "): " + e.getProgressPercentage());
    }
});
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
