---
id: document-renaming
url: search/nodejs-java/document-renaming
title: Document renaming
weight: 8
description: ""
keywords: 
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Sometimes a situation arises when an indexed document is renamed, but its contents have not changed. In this case, to save computing resources, you can notify the index about the renaming of the document, and then the document will not be reindexed during the update operation.

To notify an index about renaming a document, the [notifyIndex](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#notifyIndex(com.groupdocs.search.Notification)) method is used with the corresponding notification object as a parameter.

You should keep in mind that if an index is notified of the renaming of a document, it will not be reindexed the next time you call the [update](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#update()) method, even if its contents have changed. The following example demonstrates how to notify an index of a renamed document.

```javascript
const indexFolder = 'c:/MyIndex';
const documentFolder = 'c:/MyDocuments';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents in a document folder
index.add(documentFolder);

// Renaming a document
const oldDocumentPath = documentFolder + 'Lorem ipsum.txt';
const newDocumentPath = documentFolder + 'Lorem ipsum renamed.txt';
fs.renameSync(oldDocumentPath, newDocumentPath);

// Notifying the index about renaming
const notification = java.callStaticMethodSync(
  'com.groupdocs.search.Notification',
  'createRenameNotification',
  oldDocumentPath,
  newDocumentPath,
);
const result = index.notifyIndex(notification);
console.log('\nSuccessful rename: ' + result);

// Updating the index
// The renamed document will not be reindexed
index.update();
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
