---
id: update-index
url: search/net/update-index
title: Update index
weight: 26
description: "This article explains how to update indexed documents and how to update the version of an index created by a previous version."
keywords: update indexed documents, update index version, IndexUpdater, VersionUpdateResult, IndexedWithError
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains a description of updating indexed documents, as well as updating an index version.

## Update indexed documents

The update operation is used to reindex documents that have been changed, deleted or added to indexed folders. Changing a filter specified by the [DocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/documentfilter/) property of the [IndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/) class can also lead to a change in the list of indexed documents.

The update operation also indexes again the documents whose previous indexing was not completed. For example, a document whose indexing was interrupted by [cancellation]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-options.md#cancellation-property" >}}) is marked with the [IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/) flag and is indexed again by the next update. Note that only documents indexed from files can be indexed again: documents added from streams, structures or extracted data have to be added again. In the same way, the update indexes again the files of documents deleted when a damaged index is repaired (see [Checking index integrity]({{< ref "search/net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}})).

When updating, the same options can be specified in the instance of the [UpdateOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/updateoptions/) class, which are set in the [IndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/) class to specify indexing options. See the [Indexing options]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-options.md" >}}) page.

The following example demonstrates how to update an index using 2 threads.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentFolder);
 
// Change, delete, add documents in the document folder
// ...
 
UpdateOptions options = new UpdateOptions();
options.Threads = 2; // Setting the number of indexing threads
index.Update(options); // Updating the index
```

## Update index version

When a new version of the GroupDocs.Search library changes the format for storing the index on disk, an index created by a previous version has to be updated to the new format with the [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) class. Version 26.10 introduces a new index format: an index of a previous version, including versions 26.7 and 26.8, cannot be opened until its version is updated, and the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) constructors throw [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) for it.

The [UpdateVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/updateversion/) method updates an index as follows:

*   An index created by versions 22.7–26.8 is converted to the new format without re-indexing, so the source documents are not required.
*   An index created by versions 18.5–22.6 is updated by indexing the documents again, so the documents must be available at the paths from which they were indexed.
*   The source index is never modified. The updated index is written to another folder, which must not be the source folder, a folder that contains it or a folder inside it. The content of the target folder is deleted before the updated index is written.
*   The method returns a value of the [VersionUpdateResult](https://reference.groupdocs.com/search/net/groupdocs.search.common/versionupdateresult/) enumeration: *Updated*, *AlreadyUpToDate* (the index already has the current version) or *Unsupported* (the folder does not contain an index that can be updated).

The [IsLatestVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/islatestversion/) method checks whether a folder contains an index of the current version, and the [CanUpdateVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/canupdateversion/) method checks whether a folder contains an index of a previous version that can be updated. After a successful update, the folder containing the old version of the index may be deleted. The full rules, including the exceptions and the settings of an updated index, are described on the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

The following example demonstrates updating a previous version of an index.

**C#**

```csharp
string sourceIndexFolder = @"c:\MyOldIndex\";
string targetIndexFolder = @"c:\MyNewIndex\";
 
IndexUpdater updater = new IndexUpdater();
 
if (updater.IsLatestVersion(sourceIndexFolder))
{
    // The index has the current version and can be opened without updating
    Index index = new Index(sourceIndexFolder);
}
else if (updater.CanUpdateVersion(sourceIndexFolder))
{
    // The index of the old version does not change
    VersionUpdateResult result = updater.UpdateVersion(sourceIndexFolder, targetIndexFolder);
    if (result == VersionUpdateResult.Updated)
    {
        // Opening the updated index
        Index index = new Index(targetIndexFolder);
    }
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
