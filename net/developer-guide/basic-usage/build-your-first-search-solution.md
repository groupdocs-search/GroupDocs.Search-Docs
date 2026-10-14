---
id: build-your-first-search-solution
url: search/net/build-your-first-search-solution
title: Build your first search solution
weight: 1
description: "This article describes the basic steps of building a search solution with GroupDocs.Search for .NET: creating and opening an index, indexing documents, searching and processing search results."
keywords: create an index, open an index, index documents, search, highlight search results
productName: GroupDocs.Search for .NET
hideChildren: False
---
This brief guide describes the basic steps for creating your first search index.

## Step #1: Create new index

First of all you need to create an index. An index can be created in memory or on disk. An index created in memory is lost when your program exits unless it is saved to disk with the [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) method. In contrast, an index created on disk may be loaded in the future to continue working. Details on creating an index are described in the section [Creating an index]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/_index.md" >}}). The following example shows how to create an index on disk.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\"; // Specify the path to the index folder
Index index = new Index(indexFolder);
```

## Step #2: Open existing index

To continue working with a previously created index, it must be loaded. The constructors of the **[Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/)** class that take an index folder path load the index if it exists in the specified folder, and create a new index otherwise. An existing index is overwritten only if the *overwriteIfExists* parameter is *true*. The constructors without a folder path, **Index()** and **Index(IndexSettings)**, create a new index in memory.

The **[Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/)** class implements the [IDisposable](https://learn.microsoft.com/dotnet/api/system.idisposable) interface. When an index is no longer needed, call its **Dispose** method or use the *using* statement to release the resources used by the index. The following example shows how to load an existing index.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\"; // Specify the path to the index folder
Index index = new Index(indexFolder);
```

{{< alert style="info" >}}Since version 26.10, an index created by a previous version cannot be opened until it is updated with the [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) class; the constructor throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception). See [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}).{{< /alert >}}

## Step #3: Subscribe to index events

After creating an index, you need to add documents to the index for indexing. Indexing documents can be successful or unsuccessful for various reasons, for example, due to read errors from the disk or the presence of a password to access a document. To receive information about indexing errors, you can subscribe to the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event. To work with events, see the section [Search index events]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}). The following example continues the previous one and shows how to subscribe to the ErrorOccurred event.

**C#**

```csharp
index.Events.ErrorOccurred += (sender, args) =>
{
    Console.WriteLine(args.Message); // Writing error messages to the console
};
```

## Step #4: Add files synchronously

Document indexing can be performed synchronously or asynchronously. Synchronous indexing means that a thread that started the indexing process will be busy until the operation is completed. The following example shows how to perform indexing synchronously.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\"; // Specify path to the index folder
string documentsFolder = @"c:\MyDocuments\"; // Specify the path to a folder containing documents to search
 
Index index = new Index(indexFolder); // Creating an index in the specified folder
 
index.Add(documentsFolder); // Synchronous indexing documents from the specified folder
```

## Step #5: Add files asynchronously

More often, however, it is necessary to perform indexing asynchronously, with the ability to execute other tasks in the thread that launched the operation. A detailed description of all aspects of the indexing process is provided in the section [Indexing]({{< ref "search/net/developer-guide/advanced-usage/indexing/_index.md" >}}). The following example shows how to perform indexing asynchronously.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\"; // Specify path to the index folder
string documentsFolder = @"c:\MyDocuments\"; // Specify the path to a folder containing documents to search
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.OperationFinished += (sender, args) =>
{
    // There should be a code indicating the completion of the operation
    Console.WriteLine("Operation finished: " + args.OperationType);
};
 
// Setting the flag for asynchronous indexing
IndexingOptions options = new IndexingOptions();
options.IsAsync = true;
 
// Asynchronous indexing documents from the specified folder
// The current method terminates before the operation completes
index.Add(documentsFolder, options);
```

## Step #6: Perform search

When documents are indexed, the index is ready to handle search queries. The following types of search queries are supported: simple, fuzzy, case sensitive, boolean, phrase, faceted, with wildcards, and others. Description of search queries of various types is presented in the section [Searching]({{< ref "search/net/developer-guide/advanced-usage/searching/_index.md" >}}). The example below continues the previous examples and shows how to perform simple search in an index.

**C#**

```csharp
string query = "Einstein"; // Specify a search query
SearchResult result = index.Search(query); // Searching in the index
```

## Step #7: Use search results

When a search is completed, you need to somehow interpret a result. The result can be represented by a simple list of documents found, or the words and phrases found can be highlighted in the text of the document. For more information on processing search results, see [Search results]({{< ref "search/net/developer-guide/advanced-usage/searching/search-results.md" >}}). The example below shows how to list found documents in the console.

{{< alert style="info" >}}In the evaluation mode, only the first 10 found documents can be retrieved: the [GetFoundDocument](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/getfounddocument/) method throws [LicenseRestrictionException](https://reference.groupdocs.com/search/net/groupdocs.search.exceptions/licenserestrictionexception/) for a document number of 10 or greater. See [Evaluation limitations and licensing]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md" >}}).{{< /alert >}}

**C#**

```csharp
string query = "Einstein"; // Specify a search query

// Search in index
SearchResult result = index.Search(query);
 
// Printing the result
Console.WriteLine("Documents found: " + result.DocumentCount);
Console.WriteLine("Total occurrences found: " + result.OccurrenceCount);
for (int i = 0; i < result.DocumentCount; i++)
{
    FoundDocument document = result.GetFoundDocument(i);
    Console.WriteLine("\tDocument: " + document.DocumentInfo.FilePath);
    Console.WriteLine("\tOccurrences: " + document.OccurrenceCount);
}
```

The following example shows how to highlight search results in the text of a document. Detailed information on how to highlight search results is described in the section [Highlighting search results]({{< ref "search/net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}).

**C#**

```csharp
string query = "Einstein"; // Specify a search query

SearchResult result = index.Search(query); // Search in the index
if (result.DocumentCount > 0)
{
    FoundDocument document = result.GetFoundDocument(0); // Getting the first found document
    OutputAdapter outputAdapter = new FileOutputAdapter(OutputFormat.Html, @"c:\Highlighted.html"); // Creating the output adapter to a file
    Highlighter highlighter = new DocumentHighlighter(outputAdapter); // Creating the highlighter object
    index.Highlight(document, highlighter); // Generating output HTML formatted document with highlighted search results
}
```

## More resources

### Advanced usage topics

To learn more about search features and get familiar how to enhance your search solution, please refer to the [advanced usage section]({{< ref "search/net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
