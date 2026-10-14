---
id: search-index-events
url: search/net/search-index-events
title: Search index events
weight: 1
description: "This page contains information about the purpose and use of all index events."
keywords: index events, events, subscribing to event, track progress
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains information about the purpose and use of all index events.

## OperationFinished event

The [OperationFinished](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/operationfinished/) event occurs when an index operation completes. The [OperationType](https://reference.groupdocs.com/search/net/groupdocs.search.events/operationfinishedeventargs/operationtype/) property of the event arguments returns a value of the [OperationType](https://reference.groupdocs.com/search/net/groupdocs.search.events/operationtype/) enumeration:

*   **Indexing** - adding documents to an index;
*   **Updating** - updating documents in an index;
*   **Merging** - merging indexes;
*   **Optimizing** - optimizing an index (merging its segments);
*   **Deleting** - deleting documents from an index by paths or by document keys.

This event can be used to receive notification of the completion of an asynchronous operation. The following example demonstrates the use of the event.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.OperationFinished += (sender, args) =>
{
    // Writing operation details to the console
    Console.WriteLine("Operation finished: " + args.OperationType);
    Console.WriteLine("Message: " + args.Message);
    Console.WriteLine("Index folder: " + args.IndexFolder);
    Console.WriteLine("Time: " + args.Time);
};
 
// Indexing documents from the specified folder
IndexingOptions options = new IndexingOptions();
options.IsAsync = true; // Enabling asynchronous indexing mode
index.Add(documentsFolder, options);
```

## ErrorOccurred event

The [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event occurs when an error happens in an index. Errors in an index can be caused, for example, by file system errors or unsupported formats of indexed documents. The [IsCritical](https://reference.groupdocs.com/search/net/groupdocs.search.events/indexerroreventargs/iscritical/) property of the event arguments indicates a critical error after which the index gets the [Failed](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexstatus/) status and has to be opened again. The [CheckSegments](https://reference.groupdocs.com/search/net/groupdocs.search/index/checksegments/) method also reports the damage found and the repairs made through this event (see [Checking index integrity]({{< ref "search/net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}})). An example of receiving error notifications in the index is presented below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string query = "Einstein";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.ErrorOccurred += (sender, args) =>
{
    // Writing an error message to the console
    Console.WriteLine((args.IsCritical ? "Critical error: " : "Error: ") + args.Message);
};
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Searching in the index
SearchResult result = index.Search(query);
```

## OperationProgressChanged event

The [OperationProgressChanged](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/operationprogresschanged/) event occurs when the progress of an indexing or updating operation changes. The event arguments ([OperationProgressEventArgs](https://reference.groupdocs.com/search/net/groupdocs.search.events/operationprogresseventargs/)) contain the total number of documents to process, the numbers of processed and skipped documents, the progress percentage, as well as the key, the path and the status of the last processed document. The example below demonstrates how to track the progress of an index operation.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.OperationProgressChanged += (sender, args) =>
{
    Console.WriteLine("Last processed: " + args.LastDocumentPath);
    Console.WriteLine("Result: " + args.LastDocumentStatus);
    Console.WriteLine("Processed documents: " + args.ProcessedDocuments);
    Console.WriteLine("Skipped documents: " + args.SkippedDocuments);
    Console.WriteLine("Total documents: " + args.TotalDocuments);
    Console.WriteLine("Progress percentage: " + args.ProgressPercentage);
};
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
```

## OptimizationProgressChanged event

The [OptimizationProgressChanged](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/optimizationprogresschanged/) event occurs when the progress of an index optimization operation changes. The example below demonstrates how to track the progress of the index optimization operation.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);

// Subscribing to the event
index.Events.OptimizationProgressChanged += (sender, args) =>
{
    Console.WriteLine("Processed segments: " + args.ProcessedSegments);
    Console.WriteLine("Total segments: " + args.TotalSegments);
    Console.WriteLine("Progress percentage: " + args.ProgressPercentage);
};

// Optimizing the index
index.Optimize();
```

## PasswordRequired event

The [PasswordRequired](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/passwordrequired/) event occurs when an index requires a password to open a document. An example of processing this event is presented below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.PasswordRequired += (sender, args) =>
{
    if (args.DocumentFullPath.EndsWith("ProtectedDocument.pdf", StringComparison.InvariantCultureIgnoreCase))
    {
        args.Password = "123456";
    }
};
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
```

## FileIndexing event

The [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event occurs immediately before the start of indexing a document. This event can be used for

*   Skipping indexing of the current document (see also [Document filtering during indexing]({{< ref "search/net/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}}) page);
*   Specifying the encoding of the current text document (see also [Text file encoding detection]({{< ref "search/net/developer-guide/advanced-usage/indexing/text-file-encoding-detection.md" >}}) page);
*   Specifying a custom text extractor for the current document (see also [Custom text extractors]({{< ref "search/net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}) page);
*   Setting additional arbitrary fields for the current document;
*   Setting the [attributes](https://reference.groupdocs.com/search/net/groupdocs.search.events/fileindexingeventargs/attributes/) of the current document (see also [Document attributes]({{< ref "search/net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) page);
*   Specifying a custom [word splitter](https://reference.groupdocs.com/search/net/groupdocs.search.events/fileindexingeventargs/wordsplitter/) for the current document (see also [Custom text segmenter]({{< ref "search/net/developer-guide/advanced-usage/indexing/custom-text-segmenter.md" >}}) page).

The following example demonstrates how to add additional fields to documents ending in "Protected.pdf" and how to skip indexing documents containing "important" text in their paths.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.FileIndexing += (sender, args) =>
{
    if (args.DocumentFullPath.EndsWith("Protected.pdf", StringComparison.InvariantCultureIgnoreCase))
    {
        args.AdditionalFields = new DocumentField[]
        {
            new DocumentField("Tags", "Protected")
        };
    }
    if (args.DocumentFullPath.ToLowerInvariant().Contains("important"))
    {
        args.SkipIndexing = true;
    }
};
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
```

## StatusChanged event

The [StatusChanged](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/statuschanged/) event occurs when an index status changes. The new status is available in the [Status](https://reference.groupdocs.com/search/net/groupdocs.search.events/baseindexeventargs/status/) property of the event arguments. The following example demonstrates how to use this event to notify the completion of an index operation.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.StatusChanged += (sender, args) =>
{
    if (args.Status == IndexStatus.Ready || args.Status == IndexStatus.Failed)
    {
        // A notification of the operation completion should be here
    }
};
 
// Setting the flag for asynchronous indexing
IndexingOptions options = new IndexingOptions();
options.IsAsync = true;
 
// Asynchronous indexing documents from the specified folder
// The method terminates before the operation completes
index.Add(documentsFolder, options);
```

## SearchPhaseCompleted event

The [SearchPhaseCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/searchphasecompleted/) event occurs when a phase (or stage) of a search operation in an index completes. This event is used to study intermediate search results when tuning search queries. Information on the phases of different types of search is presented on the page [Search flow]({{< ref "search/net/developer-guide/advanced-usage/searching/search-flow.md" >}}). The following example demonstrates the use of this event.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Subscribing to the event
index.Events.SearchPhaseCompleted += (sender, args) =>
{
    Console.WriteLine("Search phase: " + args.SearchPhase);
    Console.WriteLine("Words: " + args.Words.Length);
    for (int i = 0; i < args.Words.Length; i++)
    {
        Console.WriteLine("\t" + args.Words[i]);
    }
    Console.WriteLine();
};
 
SearchOptions options = new SearchOptions();
options.UseSynonymSearch = true;
options.UseWordFormsSearch = true;
options.FuzzySearch.Enabled = true;
options.UseHomophoneSearch = true;
SearchResult result = index.Search("Einstein", options);
```

## ImagePreparing event

The [ImagePreparing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/imagepreparing/) event occurs immediately before adding indexed image to an index. The event can be used, for example, to save an image separately from its containing document, since it provides an image data stream. The following example demonstrates the use of this event.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index
Index index = new Index(indexFolder);

// Subscribing to the event
index.Events.ImagePreparing += (sender, args) =>
{
    Console.WriteLine("Document: " + args.DocumentKey);
    Console.WriteLine("Image inner path: " + string.Join("/", args.InnerPath));
    Console.WriteLine("Image index: " + args.ImageIndex);
    Console.WriteLine("Image frames: " + args.ImageFrames.Length);
};

// Indexing files
index.Add(documentsFolder);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
