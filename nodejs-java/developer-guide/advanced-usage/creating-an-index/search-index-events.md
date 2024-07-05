---
id: search-index-events
url: search/nodejs-java/search-index-events
title: Search index events
weight: 1
description: "The OperationFinished event occurs when an index operation completes – indexing, updating, merging, or optimizing (segment merging)"
keywords: indexing, updating, merging,segment merging
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page contains information about the purpose and use of all index events.

## OperationFinished event

The [OperationFinished](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#OperationFinished) event occurs when an index operation completes – indexing, updating, merging, deleting, or optimizing (segment merging). This event can be used to receive notification of the completion of an asynchronous operation. The following example demonstrates the use of the event.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Subscribing to the event
index.getEvents().OperationFinished.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      console.log('Operation finished: ' + args.getOperationType());
      console.log('Message: ' + args.getMessage());
      console.log('Index folder: ' + args.getIndexFolder());
      const df = new SimpleDateFormat('MM/dd/yyyy HH:mm:ss');
      console.log('Time: ' + df.format(args.getTime()));
    },
  }),
);

// Indexing documents from the specified folder
index.add(documentsFolder);
```

## ErrorOccurred event

The [ErrorOccured](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#ErrorOccurred) event occurs when an error happens in an index. Errors in an index can be caused, for example, by file system errors or unsupported formats of indexed documents. An example of receiving error notifications in the index is presented below.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';
const query = 'Einstein';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Subscribing to the event
index.getEvents().ErrorOccurred.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      console.log(args.getMessage());
    },
  }),
);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Searching in the index
const result = index.search(query);
```

## OperationProgressChanged event

The [OperationProgressChanged](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#OperationProgressChanged) event occurs when the progress of an indexing or updating operation changes. The example below demonstrates how to track the progress of an index operation.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Subscribing to the event
index.getEvents().OperationProgressChanged.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      console.log('Last processed: ' + args.getLastDocumentPath());
      console.log('Result: ' + args.getLastDocumentStatus());
      console.log('Processed documents: ' + args.getProcessedDocuments());
      console.log('Progress percentage: ' + args.getProgressPercentage());
    },
  }),
);

// Indexing documents from the specified folder
index.add(documentsFolder);
```

## OptimizationProgressChanged event

The [OptimizationProgressChanged](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#OptimizationProgressChanged) event occurs when the progress of an index optimization operation changes. The example below demonstrates how to track the progress of the index optimization operation.

```javascript
const indexFolder = 'c:/MyIndex/';

// Opening an index
Index index = new Index(indexFolder);

// Subscribing to the event
index.getEvents().OptimizationProgressChanged.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      console.log();
      console.log('Processed segments: ' + args.getProcessedSegments());
      console.log('Total segments: ' + args.getTotalSegments());
      console.log('Progress percentage: ' + args.getProgressPercentage());
    },
  }),
);

index.optimize();
```

## PasswordRequired event

The [PasswordRequired](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#PasswordRequired) event occurs when an index requires a password to open a document. An example of processing this event is presented below.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Subscribing to the event
index.getEvents().PasswordRequired.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      if (args.getDocumentFullPath().endsWith('English.docx')) {
        args.setPassword('123456');
      } else if (args.getDocumentFullPath().endsWith('Lorem ipsum.docx')) {
        args.setPassword('123456');
      }
    },
  }),
);

// Indexing documents from the specified folder
index.add(documentsFolder);
```

## FileIndexing event

The [FileIndexing](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#FileIndexing) event occurs immediately before the start of indexing a document. This event can be used for

*   Skipping indexing of the current document (see also [Document filtering during indexing]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}}) page);
*   Specifying the encoding of the current text document (see also [Text file encoding detection]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/text-file-encoding-detection.md" >}}) page);
*   Specifying a custom text extractor for the current document (see also [Custom text extractors]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}) page);
*   Setting additional arbitrary fields for the current document.

The following example demonstrates how to add additional fields to documents ending in "Protected.pdf" and how to skip indexing documents containing "important" text in their paths.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Subscribing to the event
const additionalFields = java.newArray('com.groupdocs.search.common.DocumentField', [
  new groupdocs.search.DocumentField('Tags', 'Lorem'),
]);
index.getEvents().FileIndexing.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      if (args.getDocumentFullPath().endsWith('Lorem ipsum.docx')) {
        args.setAdditionalFields(additionalFields);
        console.log('Added field to: ' + args.getDocumentFullPath());
      }
      if (!args.getDocumentFullPath().toLowerCase().includes('lorem')) {
        args.setSkipIndexing(true);
        console.log('Skipped: ' + args.getDocumentFullPath());
      }
    },
  }),
);

// Indexing documents from the specified folder
index.add(documentsFolder);
```

## StatusChanged event

The [StatusChanged](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#StatusChanged) event occurs when an index status changes. The following example demonstrates how to use this event to notify the completion of an index operation.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Subscribing to the event
index.getEvents().StatusChanged.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      if (
        String(args.getStatus()) == String(groupdocs.search.IndexStatus.Ready) ||
        String(args.getStatus()) == String(groupdocs.search.IndexStatus.Failed)
      ) {
        // A notification of the operation completion should be here
        console.log('Operation finished!');
      }
    },
  }),
);

// Setting the flag for asynchronous indexing
const options = new groupdocs.search.IndexingOptions();
options.setAsync(true);

// Asynchronous indexing documents from the specified folder
// The method terminates before the operation completes
index.add(documentsFolder, options);
```

## SearchPhaseCompleted event

The [SearchPhaseCompleted](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#SearchPhaseCompleted) event occurs when a phase (or stage) of a search operation in an index completes. This event is used to study intermediate search results when tuning search queries. Information on the phases of different types of search is presented on the page [Search flow]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/search-flow.md" >}}). The following example demonstrates the use of this event.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Subscribing to the event
index.getEvents().SearchPhaseCompleted.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      console.log('Search phase: ' + args.getSearchPhase());
      console.log('Words: ' + args.getWords().length);
      const words = args.getWords();
      for (let i = 0; i <= words.length; i++) {
        const word = words[i];
        console.log('\t' + word);
      }
      console.log();
    },
  }),
);

const options = new groupdocs.search.SearchOptions();
options.setUseSynonymSearch(true);
options.setUseWordFormsSearch(true);
options.getFuzzySearch().setEnabled(true);
options.setUseHomophoneSearch(true);
const result = index.search('buy', options);
```

## ImagePreparing event

The [ImagePreparing](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#ImagePreparing) event occurs immediately before adding indexed image to an index. The event can be used, for example, to save an image separately from its containing document, since it provides an image data stream. The following example demonstrates the use of this event.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index
const index = new groupdocs.search.Index(indexFolder, true);

// Subscribing to the event
index.getEvents().ImagePreparing.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      console.log('Document: ' + args.getDocumentKey());
      console.log('Image index: ' + args.getImageIndex());
      console.log('Image frames: ' + args.getImageFrames().length);
    },
  }),
);

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
