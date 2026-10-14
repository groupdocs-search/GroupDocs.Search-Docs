---
id: search-index-location
url: search/net/search-index-location
title: Search index location
weight: 0
description: "This page describes how to create an index on disk or in memory, how to load an index into memory and how to save an index to another folder."
keywords: index location, in-memory index, index on disk, on-disk index
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page describes how to create an index on disk or in memory, how to load an index into memory and how to save an index to another folder.

The GroupDocs.Search for .NET search index can be created and stored on disk or entirely in RAM. All changes to the on-disk index are automatically saved and available after it is loaded again. The in-memory index is faster, but uses more RAM and is completely destroyed when you finish working with it.

To create an in-memory index, you must use a constructor that does not have a path to the index folder in its parameter list. The created in-memory index can be converted to an on-disk index using the [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) method. This is useful when an in-memory index becomes too large and can no longer reside in RAM alone. After saving to disk, the index can then be loaded from disk as usual. Note that a regular index can be saved to disk in a different folder in the same way. Below is an example of creating an in-memory index and converting it to an on-disk index.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an in-memory index
Index index = new Index();

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Saving the index to disk
index.SaveTo(indexFolder);

// Closing the in-memory index
index.Dispose();

// Opening the index from disk
index = new Index(indexFolder);

// Searching in the index
string query = "focus";
SearchResult result = index.Search(query);
```

The [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) method follows these rules:

*   It throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if the specified folder is the folder of the index itself, also when the folder is specified by another path.
*   An index of the current version in the specified folder is replaced by the saved index.
*   It throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) and does not change the folder if the folder contains an index of another version (for example, an index created by a previous version) or a damaged index.

For details, see the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

To create or load an on-disk index, you must pass the path to the index folder through the constructor parameter. The created on-disk index can be converted to an in-memory index using the [LoadIntoMemoryCompletely](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) method. After the index is completely loaded into memory, no changes to it are automatically saved to disk. You must explicitly call the [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) method to save any changes to the index. The overload [LoadIntoMemoryCompletely(string, IndexSettings)](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) allows you to pass an index settings object: the loaded index uses the settings saved in it and takes only the [custom extractors](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/customextractors/) and the [logger](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/logger/) from the passed object, since they are not saved in an index. Like the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) constructors, the method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) for an index created by a previous version; such an index must be updated first (see [Update index]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md#update-index-version" >}})). Below is an example of creating an on-disk index and converting it to an in-memory index.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating a regular index on disk
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Closing the regular index
index.Dispose();

// Loading the index entirely in memory for increased performance
// The logger is not saved in the index, so it is passed with the settings
IndexSettings settings = new IndexSettings();
settings.Logger = new FileLogger(@"c:\Log.txt", 4.0);
Index inMemoryIndex = Index.LoadIntoMemoryCompletely(indexFolder, settings);

// Searching in the index
string query = "focus";
SearchResult result = inMemoryIndex.Search(query);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
