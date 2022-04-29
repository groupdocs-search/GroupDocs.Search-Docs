---
id: search-index-repository
url: search/java/search-index-repository
title: Search index repository
weight: 2
description: "The IndexRepository class can be used to combine several indexes into a group for searching all indexes at once."
keywords: combine several indexes, searching all indexes at once, searching all indexes, combine several indexes into a group
productName: GroupDocs.Search for Java
hideChildren: False
---
The [IndexRepository](https://apireference.groupdocs.com/search/java/com.groupdocs.search/IndexRepository) class can be used to combine several indexes into a group for searching all indexes at once. In addition, a single call to the [update](https://apireference.groupdocs.com/search/java/com.groupdocs.search/IndexRepository#update()) method can update all indexes in the repository. And also the index repository serves as an event hub for all indexes in it. That is, you can subscribe to any events available for the index also through the index repository. However, note that indexing documents is performed separately for each index.

To start using the index repository, indexes must be created or loaded and added to an instance of the index repository. Examples of creating and adding indexes to the repository are presented below.



```java
String indexFolder1 = "c:\\MyIndex1\\";
String indexFolder2 = "c:\\MyIndex2\\";
String documentsFolder1 = "c:\\MyDocuments1\\";
String documentsFolder2 = "c:\\MyDocuments2\\";
 
// Creating an index repository instance
IndexRepository indexRepository = new IndexRepository();
 
// Creating or loading an index and adding to the index repository
Index index1 = new Index(indexFolder1);
indexRepository.addToRepository(index1);
 
// Creating an index via the index repository by calling a single method
Index index2 = indexRepository.create(indexFolder2);
 
// Adding documents to the index 1
index1.add(documentsFolder1);
 
// Adding documents to the index 2
index2.add(documentsFolder2);
```

The following example shows how to subscribe to the repository events, update indexes in the repository, and search in the repository.



```java
String indexFolder1 = "c:\\MyIndex1\\";
String indexFolder2 = "c:\\MyIndex2\\";
String documentFolder1 = "c:\\MyDocuments1\\";
String documentFolder2 = "c:\\MyDocuments2\\";
 
// Creating an index repository instance
IndexRepository indexRepository = new IndexRepository();
 
// Subscribing to an event
indexRepository.getEvents().ErrorOccurred.add(new EventHandler<IndexErrorEventArgs>() {
    public void invoke(Object sender, IndexErrorEventArgs args) {
        System.out.println("Index: " + args.getIndexFolder());
        System.out.println("Error: " + args.getMessage());
    }
});
 
// Creating or loading an index and adding to the index repository
Index index1 = new Index(indexFolder1);
indexRepository.addToRepository(index1);
 
// Creating or loading an index and adding to the index repository
Index index2 = new Index(indexFolder2);
indexRepository.addToRepository(index2);
 
// Adding documents to the index 1
index1.add(documentFolder1);
 
// Adding documents to the index 2
index2.add(documentFolder2);
 
// Changing, deleting, adding documents to document folders
// ...
 
// Updating all indexes in the repository
indexRepository.update();
 
// Searching in the repository
SearchResult result = indexRepository.search("Einstein");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
