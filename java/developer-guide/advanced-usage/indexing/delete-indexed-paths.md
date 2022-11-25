---
id: delete-indexed-paths
url: search/java/delete-indexed-paths
title: Delete indexed paths
weight: 5
description: "GroupDocs.Search for Java supports the ability to remove indexed files and folders from an index. Only files or folders that were explicitly added to the index can be deleted."
keywords: remove indexed files, added to the index
productName: GroupDocs.Search for Java
hideChildren: False
---
GroupDocs.Search supports the ability to remove indexed files and folders from an index. Only files or folders that were explicitly added to the index can be deleted. It is not possible to remove a file or folder from the index that is a child of the indexed folder. To delete files and folders inside indexed paths, use the document filter (see [Document filtering during indexing]({{< ref "search/java/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}})). To get a list of indexed paths, use the [getIndexedPaths](https://reference.groupdocs.com/search/java/com.groupdocs.search/Index#getIndexedPaths()) method of the [Index](https://reference.groupdocs.com/search/java/com.groupdocs.search/Index) class.

The following example shows how to remove indexed paths from an index.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder1 = "c:\\MyDocuments\\";
String documentsFolder2 = "c:\\MyDocuments2\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folders
index.add(documentsFolder1);
index.add(documentsFolder2);
 
// Getting indexed paths from the index
String[] indexedPaths1 = index.getIndexedPaths();
 
// Writing indexed paths to the console
System.out.println("Indexed paths:");
for (String path : indexedPaths1) {
    System.out.println("\t" + path);
}
 
// Deleting indexed path from the index
DeleteResult deleteResult = index.delete(new String[] { documentsFolder1 }, new UpdateOptions());
 
// Getting indexed paths after deletion
String[] indexedPaths2 = index.getIndexedPaths();
System.out.println("\nDeleted paths: " + deleteResult.getSuccessCount());
 
System.out.println("\nIndexed paths:");
for (String path : indexedPaths2) {
    System.out.println("\t" + path);
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
