---
id: logging
url: search/net/logging
title: Logging
weight: 19
description: "This article explains how to create and assign a logger of an index and how to implement a custom logger."
keywords: logging, logger, FileLogger, ILogger, custom logger
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains information on creating and assigning a logger of an index, as well as on the implementation of a custom logger.

## Use of standard file logger

In order to save information about events and errors in an index into a file, you should use the standard file logger or your own custom logger. It is important to remember that each time you open an existing index, you must create and assign an instance of the logger again, because the logger is not saved. The logger receives, among other things, the messages that the index reports while it is being opened, for example, about an interrupted indexing operation (see [Checking index integrity]({{< ref "search/net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}})). The same code can be used to create a new index and to open an existing one: when an existing index is opened, the logger is taken from the index settings passed to the constructor, and the other index settings are loaded from the index.

The following example demonstrates the use of the standard file logger. The constructor parameters of the [FileLogger](https://reference.groupdocs.com/search/net/groupdocs.search.common/filelogger/) class are the path to the log file and its maximum size in MB. The maximum size should be in the range from 0.1 to 1000 MB; the value is not checked. When the log file exceeds the maximum size, the older part of the log is removed, and the most recent half of the maximum size is kept.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string query = "Einstein";
string logPath = @"c:\Log.txt";
 
IndexSettings settings = new IndexSettings();
settings.Logger = new FileLogger(logPath, 4.0); // Specifying the path to the log file and a maximum length of 4 MB
 
Index index = new Index(indexFolder, settings); // Creating or loading an index from the specified folder
 
index.Add(documentsFolder); // Indexing documents from the specified folder
 
SearchResult result = index.Search(query); // Search in index
```

## Implementing custom logger

Sometimes you may need to implement your own logger, for example, to display the log in the console. A custom logger implements the [ILogger](https://reference.groupdocs.com/search/net/groupdocs.search.common/ilogger/) interface: the [Error](https://reference.groupdocs.com/search/net/groupdocs.search.common/ilogger/error/) method logs errors, and the [Trace](https://reference.groupdocs.com/search/net/groupdocs.search.common/ilogger/trace/) method logs events. An example implementation of such a logger is presented below.

**C#**

```csharp
public class ConsoleLogger : ILogger
{
    public ConsoleLogger()
    {
    }
 
    public void Error(string message)
    {
        Console.WriteLine("Error: " + message);
    }
 
    public void Trace(string message)
    {
        Console.WriteLine(message);
    }
}
```

The custom logger is assigned in the same way as the standard one.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

IndexSettings settings = new IndexSettings();
settings.Logger = new ConsoleLogger(); // Assigning the custom logger

Index index = new Index(indexFolder, settings); // Creating or loading an index from the specified folder
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

* [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

* [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
