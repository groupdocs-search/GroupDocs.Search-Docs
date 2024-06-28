---
id: delete-indexed-paths
url: search/nodejs-java/delete-indexed-paths
title: Delete indexed paths
weight: 5
description: "GroupDocs.Search for Node.js supports the ability to remove indexed files and folders from an index. Only files or folders that were explicitly added to the index can be deleted."
keywords: remove indexed files, added to the index
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
GroupDocs.Search supports the ability to remove indexed files and folders from an index. Only files or folders that were explicitly added to the index can be deleted. It is not possible to remove a file or folder from the index that is a child of the indexed folder. To delete files and folders inside indexed paths, use the document filter (see [Document filtering during indexing]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}})). To get a list of indexed paths, use the [getIndexedPaths](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#getIndexedPaths()) method of the [Index](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index) class.

The following example shows how to remove indexed paths from an index.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder1 = 'c:/MyDocuments/';
const documentsFolder2 = 'c:/MyDocuments2/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folders
index.add(documentsFolder1);
index.add(documentsFolder2);

// Getting indexed paths from the index
const indexedPaths1 = index.getIndexedPaths();

// Writing indexed paths to the console
console.log('Indexed paths:');
for (let i = 0; i < indexedPaths1.length; i++) {
  const indexedPath = indexedPaths1[i];
  console.log('\t' + indexedPath);
}

// Deleting index path from the index
const deleteResult = index.delete([documentsFolder1], new groupdocs.search.UpdateOptions());

// Getting indexed paths after deletion
const indexedPaths2 = index.getIndexedPaths();
console.log('\nDeleted paths: ' + deleteResult.getSuccessCount());

console.log('\nIndexed paths:');
for (let i = 0; i < indexedPaths2.length; i++) {
  const indexedPath = indexedPaths2[i];
  console.log('\t' + indexedPath);
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
