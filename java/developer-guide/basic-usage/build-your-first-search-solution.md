---
id: build-your-first-search-solution
url: search/java/build-your-first-search-solution
title: Build your first search solution
weight: 1
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
This brief guide describes the basic steps for creating your first search index.

## Step #1: Create new index

First of all you need to create an index. An index can be created in memory or on disk. An index created in memory cannot be saved after exiting your program. In contrast, an index created on disk may be loaded in the future to continue working. Details on creating an index are described in the section [Creating an index]({{< ref "search/java/developer-guide/advanced-usage/creating-an-index/_index.md" >}}). The following example shows how to create an index on disk.



```java
String indexFolder = "c:\\MyIndex\\"; // Specify the path to the index folder
Index index = new Index(indexFolder);
```

## Step #2: Open existing index

To continue working with a previously created index, it must be loaded. Each constructor of the [Index](https://apireference.groupdocs.com/search/java/com.groupdocs.search/Index) class by default loads the index, if it exists at the specified path. And only an explicit indication causes an index to be overwritten. The following example shows how to load an existing index.



```java
String indexFolder = "c:\\MyIndex\\"; // Specify the path to the index folder
Index index = new Index(indexFolder);
```

## Step #3: Subscribe to index events

After creating an index, you need to add documents to the index for indexing. Indexing documents can be successful or unsuccessful for various reasons, for example, due to read errors from the disk or the presence of a password to access a document. To receive information about indexing errors, you can subscribe to the ErrorOccurred event. To work with events, see the section [Search index events]({{< ref "search/java/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}). The following example shows how to subscribe to the ErrorOccurred event.



```java
index.getEvents().ErrorOccurred.add(new EventHandler<IndexErrorEventArgs>() {
    public void invoke(Object sender, IndexErrorEventArgs args) {
        System.out.println(args.getMessage()); // Writing error messages to the console
    }
});
```

## Step #4: Add files synchronously

Document indexing can be performed synchronously or asynchronously. Synchronous indexing means that a thread that started the indexing process will be busy until the operation is completed. The following example shows how to perform indexing synchronously.



```java
String indexFolder = "c:\\MyIndex\\"; // Specify path to the index folder
String documentsFolder = "c:\\MyDocuments\\"; // Specify the path to a folder containing documents to search
 
Index index = new Index(indexFolder); // Creating an index in the specified folder
 
index.add(documentsFolder); // Synchronous indexing documents from the specified folder
```

## Step #5: Add files asynchronously

More often, however, it is necessary to perform indexing asynchronously, with the ability to execute other tasks in the thread that launched the operation. A detailed description of all aspects of the indexing process is provided in the section [Indexing]({{< ref "search/java/developer-guide/advanced-usage/indexing/_index.md" >}}). The following example shows how to perform indexing asynchronously.



```java
String indexFolder = "c:\\MyIndex\\"; // Specify path to the index folder
String documentsFolder = "c:\\MyDocuments\\"; // Specify the path to a folder containing documents to search
 
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
// The current method terminates before the operation completes
index.add(documentsFolder, options);
```

## Step #6: Perform search

When documents are indexed, the index is ready to handle search queries. The following types of search queries are supported: simple, fuzzy, case sensitive, boolean, phrasal, faceted, with wildcards, and others. Description of search queries of various types is presented in the section [Searching]({{< ref "search/java/developer-guide/advanced-usage/searching/_index.md" >}}). The example below shows how to perform simple search in an index.



```java
String query = "Einstein"; // Specify a search query
SearchResult result = index.search(query); // Searching in the index
```

## Step #7: Use search results

When a search is completed, you need to somehow interpret a result. The result can be represented by a simple list of documents found, or the words and phrases found can be highlighted in the text of the document. For more information on processing search results, see [Search results]({{< ref "search/java/developer-guide/advanced-usage/searching/search-results.md" >}}). The example below shows how to list found documents in the console.



```java
// Search in index
SearchResult result = index.search(query);
 
// Printing the result
System.out.println("Documents found: " + result.getDocumentCount());
System.out.println("Total occurrences found: " + result.getOccurrenceCount());
for (int i = 0; i < result.getDocumentCount(); i++) {
    FoundDocument document = result.getFoundDocument(i);
    System.out.println("\tDocument: " + document.getDocumentInfo().getFilePath());
    System.out.println("\tOccurrences: " + document.getOccurrenceCount());
}
```

The following example shows how to highlight search results in the text of a document. Detailed information on how to highlight search results is described in the section [Highlighting search results]({{< ref "search/java/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}).



```java
SearchResult result = index.search(query); // Search in the index
if (result.getDocumentCount() > 0)
{
    FoundDocument document = result.getFoundDocument(0); // Getting the first found document
    OutputAdapter outputAdapter = new FileOutputAdapter("c:\\Highlighted.html"); // Creating the output adapter to a file
    HtmlHighlighter highlighter = new HtmlHighlighter(outputAdapter); // Creating the highlighter object
    index.highlight(document, highlighter); // Generating output HTML formatted document with highlighted search results
}
```

## More resources

### Advanced usage topics

To learn more about search features and get familiar how to enhance your search solution, please refer to the [advanced usage section]({{< ref "search/java/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
