---
id: search-by-chunks
url: search/nodejs-java/search-by-chunks
title: Search by chunks
weight: 18
description: "This article gives the knowledge about the ability to perform search by chunks using Java search API."
keywords: Search by chunks
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
The GroupDocs.Search API provides the ability to perform search by chunks. This means that in one call to the [search](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#search(java.lang.String,%20com.groupdocs.search.options.SearchOptions)) method of the [Index](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index) class, only in one index segment search is performed. This feature becomes relevant when searching in large indexes containing tens and hundreds of thousands of documents.

When performing search by chunks, the [search](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#search(java.lang.String,%20com.groupdocs.search.options.SearchOptions)) method is first called with the chunk search flag set to true via [setChunkSearch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setChunkSearch(boolean)) method of the [SearchOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions) class. And then the search in each subsequent segment is performed using the [searchNext](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#searchNext(com.groupdocs.search.common.ChunkSearchToken)) method with passing [ChunkSearchToken](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/ChunkSearchToken) of the next chunk as an argument.

The following example demonstrates the search by chunks.

```javascript
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
String query = "Einstein";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Creating a search options instance
SearchOptions options = new SearchOptions();
options.setChunkSearch(true); // Enabling the search by chunks
 
// Starting the search by chunks
SearchResult result = index.search(query, options);
System.out.println("Document count: " + result.getDocumentCount());
System.out.println("Occurrence count: " + result.getOccurrenceCount());
 
// Continuing the search by chunks
while (result.getNextChunkSearchToken() != null) {
    result = index.searchNext(result.getNextChunkSearchToken());
    System.out.println("Document count: " + result.getDocumentCount());
    System.out.println("Occurrence count: " + result.getOccurrenceCount());
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
