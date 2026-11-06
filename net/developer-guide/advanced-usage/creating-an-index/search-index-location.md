---
id: search-index-location
url: search/net/search-index-location
title: Search index location
weight: 0
description: "This page describes how to create an index on disk or in memory, how to load an index into memory and how to save an index to another folder, and the rules of using an index folder."
keywords: index location, in-memory index, index on disk, on-disk index, index folder, dispose index
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page describes how to create an index on disk or in memory, how to load an index into memory and how to save an index to another folder, and the rules of using an index folder.

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
*   Since version 26.11, it throws [IOException](https://learn.microsoft.com/dotnet/api/system.io.ioexception) and does not change the folder if another [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) object uses the folder, [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if the index is performing another operation, and [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception), before the folder is changed, if a file of the index is missing. An index with the [Failed](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexstatus/) status is saved in the state that it has worked with, without the change that has failed.

For details, see the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

To create or load an on-disk index, you must pass the path to the index folder through the constructor parameter. The created on-disk index can be converted to an in-memory index using the [LoadIntoMemoryCompletely](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) method. After the index is completely loaded into memory, no changes to it are automatically saved to disk. You must explicitly call the [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) method to save any changes to the index. The overload [LoadIntoMemoryCompletely(string, IndexSettings)](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) allows you to pass an index settings object: the loaded index uses the settings saved in it and takes only the [custom extractors](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/customextractors/) and the [logger](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/logger/) from the passed object, since they are not saved in an index. Like the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) constructors, the method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) for an index created by a previous version; such an index must be updated first (see [Update index]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md#update-index-version" >}})). Since version 26.11, an index loaded into memory does not use its folder: it writes nothing to the folder and does not prevent an [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) object from opening it (see [Index folder](#index-folder) below). Below is an example of creating an on-disk index and converting it to an in-memory index.

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

## Index folder

An index on disk is created or opened by an [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) constructor that takes the path to the index folder. If the folder contains an index, the index is opened. Otherwise, or if the *overwriteIfExists* parameter is *true*, a new index is created; with *overwriteIfExists* set to *true*, all files and subfolders of the folder are deleted first. Since version 26.11, without *overwriteIfExists*, a new index is created only in a folder that does not exist or is empty, or that contains only the files of an index that cannot be opened, for example of an index whose creation has failed. A folder that contains no index but other files or subfolders is refused with [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception), and nothing in it is deleted; previously, its content was deleted. To create an index in such a folder and delete its content, set *overwriteIfExists* to *true*. Do not keep other data in the folder of an index.

Since version 26.11, an index folder is used by one [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) object at a time, in this process or in another one. A constructor that creates or opens an index in a folder that another Index object uses throws [IOException](https://learn.microsoft.com/dotnet/api/system.io.ioexception), and so do the [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) and [IndexUpdater.UpdateVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/updateversion/) methods into such a folder and the [Create](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/create/) and [AddToRepository](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/addtorepository/) methods of an index repository with such a folder; the folder is not changed. The folder is released when the Index object is disposed. Therefore, dispose of an index, for example with a *using* statement, before its folder is used again: an Index object that is not disposed keeps its folder until the garbage collector finalizes it, which may not happen before the process ends. An index loaded by the [LoadIntoMemoryCompletely](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) method does not use its folder. Previous versions of the library do not observe this rule, so all applications that use the same index folder must be upgraded together. The following example demonstrates how to dispose of an index before its folder is opened again.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index and indexing documents; the index is disposed at the end of the using block
using (Index index = new Index(indexFolder))
{
    index.Add(documentsFolder);
}

// The folder is released, so the index can be opened again
using (Index index = new Index(indexFolder))
{
    SearchResult result = index.Search("Einstein");
}
```

On Windows, one file of the folder of an open index cannot be read by other programs, so a program that copies the folder, for example a backup program, reports that file; it need not be copied. To copy an open index, use the [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) method.

An index that cannot be opened is refused with an exception: [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) for an index of a previous version that must be updated, of a version that cannot be updated or, since version 26.11, of a newer version; [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception) or [FileNotFoundException](https://learn.microsoft.com/dotnet/api/system.io.filenotfoundexception) for a damaged index. Since version 26.11, an index that has lost the files that record its changes is a damaged index (InvalidDataException). For details, see [Opening an index]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#opening-an-index" >}}). The errors that occur while an index is opened but do not prevent its opening, for example a damaged dictionary of synonyms, which then gets its default content, are reported before a handler of the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event can be added: they are written to the [log]({{< ref "search/net/developer-guide/advanced-usage/indexing/logging.md" >}}) of the index and are listed in the [errors](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/errors/) of the next indexing report.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
