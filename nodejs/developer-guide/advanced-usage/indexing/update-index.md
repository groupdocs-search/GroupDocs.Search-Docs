---
id: update-index
url: search/nodejs-java/update-index
title: Update index
weight: 22
description: "This article explains that how to update indexed documents, as well as updating an index version in Java."
keywords: update indexed documents, updating an index version
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page contains a description of updating indexed documents, as well as updating an index version.

## Update indexed documents

The update operation is used to reindex documents that have been changed, deleted or added to indexed folders. Changing a filter specified by the argument of the [setDocumentFilter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setDocumentFilter(com.groupdocs.search.DocumentFilter)) method of the [IndexSettings](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings) class can also lead to a change in the list of indexed documents.

When updating, the same options can be specified in the instance of the [UpdateOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/UpdateOptions) class, which are set in the [IndexingOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/IndexingOptions) class to specify indexing options. See the [Indexing options]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/indexing-options.md" >}}) page.

The following example demonstrates how to update an index using 2 threads.

```javascript
String indexFolder = "c:\\MyIndex\\";
String documentFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentFolder);
 
// Change, delete, add documents in the document folder
// ...
 
UpdateOptions options = new UpdateOptions();
options.setThreads(2); // Setting the number of indexing threads
index.update(options); // Updating the index
```

## Update index version

Sometimes when a new version of the GroupDocs.Search library is released, the format for storing the index on disk changes. In this case, you also need to update the index. However, updating the index version is different. To do this, use the [IndexUpdater](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexUpdater) class. Without updating the index version, loading the index of the previous version will fail.

When the index version is updated, the documents are reindexed and saved in a folder different from the original in the new format. However, the index of old version does not change. The folder containing the old version of the index may be deleted after the update. The following example demonstrates updating a previous version of an index.

```javascript
String sourceIndexFolder = "c:\\MyOldIndex\\";
String targetIndexFolder = "c:\\MyNewIndex\\";
 
IndexUpdater updater = new IndexUpdater();
 
if (updater.canUpdateVersion(sourceIndexFolder)) {
    // The index of old version does not change
    int result = updater.updateVersion(sourceIndexFolder, targetIndexFolder);
    // Class VersionUpdateResult contains all possible result values
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
