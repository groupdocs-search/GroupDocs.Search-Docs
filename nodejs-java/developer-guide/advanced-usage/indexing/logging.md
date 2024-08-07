---
id: logging
url: search/nodejs-java/logging
title: Logging
weight: 16
description: ""
keywords: 
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page contains information on creating and assigning a logger of an index, as well as on the implementation of a custom logger.

## Use of standard file logger

In order to save information about events and errors in an index into a file, you should use the standard file logger or your own custom logger. It is important to remember that each time you open an existing index, you must create and assign an instance of the logger again, because the logger is not saved. The good news is that the same code can be used to create a new index and open an existing one. In this case, when opening an existing index, a logger will be used from the index settings passed to the constructor, the remaining index settings will be loaded from disk.

The following example demonstrates the use of the standard file logger. The constructor parameters of this logger are the path to the log file and its maximum size in MB.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';
const query = 'Einstein';
const logPath = 'c:/Log.txt';

const settings = new groupdocs.search.IndexSettings();
settings.setLogger(new groupdocs.search.FileLogger(logPath, 4.0)); // Specifying the path to the log file and a maximum length of 4 MB

const index = new groupdocs.search.Index(indexFolder, settings); // Creating or loading an index from the specified folder

index.add(documentsFolder); // Indexing documents from the specified folder

const result = index.search(query); // Search in index
```

## Implementing custom logger

Sometimes you may need to implement your own logger, for example, to display the log in the console. An example implementation of such a logger is presented below.

```javascript
// Implementing a custom console logger
const consoleLogger = java.newProxy('com.groupdocs.search.common.ILogger', {
  error: function (message) {
    console.log('Error: ' + message);
  },
  trace: function (message) {
    console.log(message);
  },
});

settings.setLogger(consoleLogger); // Setting the custom logger
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
