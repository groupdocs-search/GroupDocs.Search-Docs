---
id: search-index-events
url: search/java/search-index-events
title: Search index events
weight: 1
description: "The OperationFinished event occurs when an index operation completes – indexing, updating, merging, or optimizing (segment merging)"
keywords: indexing, updating, merging,segment merging
productName: GroupDocs.Search for Java
hideChildren: False
---
This page contains information about the purpose and use of all index events.

## OperationFinished event

The [OperationFinished](https://apireference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#OperationFinished) event occurs when an index operation completes – indexing, updating, merging, deleting, or optimizing (segment merging). This event can be used to receive notification of the completion of an asynchronous operation. The following example demonstrates the use of the event.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.getEvents().OperationFinished.add(new EventHandler<OperationFinishedEventArgs>() {
    public void invoke(Object sender, OperationFinishedEventArgs args) {
        System.out.println("Operation finished: " + args.getOperationType());
        System.out.println("Message: " + args.getMessage());
        System.out.println("Index folder: " + args.getIndexFolder());
        SimpleDateFormat df = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
        System.out.println("Time: " + df.format(args.getTime()));
    }
});
 
// Indexing documents from the specified folder
index.add(documentsFolder);
```

## ErrorOccurred event

The [ErrorOccured](https://apireference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#ErrorOccurred) event occurs when an error happens in an index. Errors in an index can be caused, for example, by file system errors or unsupported formats of indexed documents. An example of receiving error notifications in the index is presented below.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
String query = "Einstein";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.getEvents().ErrorOccurred.add(new EventHandler<IndexErrorEventArgs>() {
    public void invoke(Object sender, IndexErrorEventArgs args) {
        System.out.println(args.getMessage());
    }
});
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Searching in the index
SearchResult result = index.search(query);
```

## OperationProgressChanged event

The [OperationProgressChanged](https://apireference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#OperationProgressChanged) event occurs when the progress of an index operation changes. The example below demonstrates how to track the progress of an index operation.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.getEvents().OperationProgressChanged.add(new EventHandler<OperationProgressEventArgs>() {
    public void invoke(Object sender, OperationProgressEventArgs args) {
        System.out.println("Last processed: " + args.getLastDocumentPath());
        System.out.println("Result: " + args.getLastDocumentStatus());
        System.out.println("Processed documents: " + args.getProcessedDocuments());
        System.out.println("Progress percentage: " + args.getProgressPercentage());
    }
});
 
// Indexing documents from the specified folder
index.add(documentsFolder);
```

## PasswordRequired event

The [PasswordRequired](https://apireference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#PasswordRequired) event occurs when an index requires a password to open a document. An example of processing this event is presented below.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.getEvents().PasswordRequired.add(new EventHandler<PasswordRequiredEventArgs>() {
    public void invoke(Object sender, PasswordRequiredEventArgs args) {
        if (args.getDocumentFullPath().endsWith("ProtectedDocument.pdf")) {
            args.setPassword("123456");
        }
    }
});
 
// Indexing documents from the specified folder
index.add(documentsFolder);
```

## FileIndexing event

The [FileIndexing](https://apireference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#FileIndexing) event occurs immediately before the start of indexing a document. This event can be used for

*   Skipping indexing of the current document (see also [Document filtering during indexing]({{< ref "search/java/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}}) page);
*   Specifying the encoding of the current text document (see also [Text file encoding detection]({{< ref "search/java/developer-guide/advanced-usage/indexing/text-file-encoding-detection.md" >}}) page);
*   Specifying a custom text extractor for the current document (see also [Custom text extractors]({{< ref "search/java/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}) page);
*   Setting additional arbitrary fields for the current document.

The following example demonstrates how to add additional fields to documents ending in "Protected.pdf" and how to skip indexing documents containing "important" text in their paths.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.getEvents().FileIndexing.add(new EventHandler<FileIndexingEventArgs>() {
    public void invoke(Object sender, FileIndexingEventArgs args) {
        if (args.getDocumentFullPath().endsWith("Protected.pdf")) {
            args.setAdditionalFields(new DocumentField[] {
                new DocumentField("Tags", "Protected")
            });
        }
        if (!args.getDocumentFullPath().toLowerCase().contains("important")) {
            args.setSkipIndexing(true);
        }
    }
});
 
// Indexing documents from the specified folder
index.add(documentsFolder);
```

## StatusChanged event

The [StatusChanged](https://apireference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#StatusChanged) event occurs when an index status changes. The following example demonstrates how to use this event to notify the completion of an index operation.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.getEvents().StatusChanged.add(new EventHandler<BaseIndexEventArgs>() {
    public void invoke(Object sender, BaseIndexEventArgs args) {
        if (args.getStatus() != IndexStatus.InProgress) {
            // A notification of the operation completion should be here
        }
    }
});
 
// Setting the flag for asynchronous indexing
IndexingOptions options = new IndexingOptions();
options.setAsync(true);
 
// Asynchronous indexing documents from the specified folder
// The method terminates before the operation completes
index.add(documentsFolder, options);
```

## SearchPhaseCompleted event

The [SearchPhaseCompleted](https://apireference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#SearchPhaseCompleted) event occurs when a phase (or stage) of a search operation in an index completes. This event is used to study intermediate search results when tuning search queries. Information on the phases of different types of search is presented on the page [Search flow]({{< ref "search/java/developer-guide/advanced-usage/searching/search-flow.md" >}}). The following example demonstrates the use of this event.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Subscribing to the event
index.getEvents().SearchPhaseCompleted.add(new EventHandler<SearchPhaseEventArgs>() {
    public void invoke(Object sender, SearchPhaseEventArgs args) {
        System.out.println("Search phase: " + args.getSearchPhase());
        System.out.println("Words: " + args.getWords().length);
    }
});
 
SearchOptions options = new SearchOptions();
options.setUseSynonymSearch(true);
options.setUseWordFormsSearch(true);
options.getFuzzySearch().setEnabled(true);
options.setUseHomophoneSearch(true);
SearchResult result = index.search("Einstein", options);
```

## ImagePreparing event

The [ImagePreparing](https://apireference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#ImagePreparing) event occurs immediately before adding indexed image to an index. The event can be used, for example, to save an image separately from its containing document, since it provides an image data stream. The following example demonstrates the use of this event.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";

// Creating an index
Index index = new Index(indexFolder);

// Subscribing to the event
index.getEvents().ImagePreparing.add(new EventHandler<ImagePreparingEventArgs>() {
    public void invoke(Object sender, ImagePreparingEventArgs args) {
        System.out.println("Document: " + args.getDocumentKey());
        System.out.println("Image inner path: " + String.join("/", args.getInnerPath()));
        System.out.println("Image index: " + args.getImageIndex());
        System.out.println("Image frames: " + args.getImageFrames().length);
    }
});

// Indexing files
index.add(documentsFolder);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
