---
id: document-filtering-in-search-result
url: search/java/document-filtering-in-search-result
title: Document filtering in search result
weight: 4
description: "This article gives the knowledge that how the document filters used during the search using Java search API."
keywords: Document filtering, Document filtering in search result
productName: GroupDocs.Search for Java
hideChildren: False
---
This page contains a description of the document filters used during the search.

## Setting a filter

To specify which of the documents found should be returned as a result of the search, the [setSearchDocumentFilter](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/SearchOptions#setSearchDocumentFilter(com.groupdocs.search.options.ISearchDocumentFilter)) method of the [SearchOptions](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/SearchOptions) class is used. If the document found does not match a filter passed to this method as an argument, the document will not be returned. The default value is null, which means that all documents found will be returned. The following example shows how to set a document filter for searching.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Creating a search options object
SearchOptions options = new SearchOptions();
options.setSearchDocumentFilter(SearchDocumentFilter.createFileExtension(".txt")); // Setting a document filter
 
// Search in the index
// Only text documents will be returned as the result of the search
SearchResult result = index.search("relativity", options);
```

## File path filters

The first supported type of search document filters allows you to set a regular expression for getting those documents whose full paths match the specified pattern. This type of filters uses the **java.util.regex.Pattern** class to compare with a pattern.



```java
// The filter returns only files that contain the word 'Einstein' in their paths, not case sensitive
ISearchDocumentFilter filter = SearchDocumentFilter.createFilePathRegularExpression("Einstein", Pattern.CASE_INSENSITIVE);
```

## File extension filter

The next supported type of search document filters allows you to specify a list of valid file extensions for indexing.



```java
// This filter returns only FB2 and EPUB documents
ISearchDocumentFilter filter = SearchDocumentFilter.createFileExtension(".fb2", ".epub");
```

## Attribute filter

The next supported type of search document filters allows you to search only those documents with which the specified text attribute is associated. You can learn more about attributes on the [Document attributes]({{< ref "search/java/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) page.



```java
// This filter returns only documents that have attribute "main"
ISearchDocumentFilter filter = SearchDocumentFilter.createAttribute("main");
```

## Combining filters

Search document filters can be combined using composite filters AND, OR, NOT. The following example shows how to combine search document filters.



```java
// Creating an AND composite filter that returns all FB2 and EPUB documents that have the word 'Einstein' in their full paths
ISearchDocumentFilter filter1 = SearchDocumentFilter.createFilePathRegularExpression("Einstein", Pattern.CASE_INSENSITIVE);
ISearchDocumentFilter filter2 = SearchDocumentFilter.createFileExtension(".fb2", ".epub");
ISearchDocumentFilter andFilter = SearchDocumentFilter.createAnd(filter1, filter2);
 
// Creating an OR composite filter that returns all DOC, DOCX, PDF and all documents that have the word Einstein in their full paths
ISearchDocumentFilter filter3 = SearchDocumentFilter.createFilePathRegularExpression("Einstein", Pattern.CASE_INSENSITIVE);
ISearchDocumentFilter filter4 = SearchDocumentFilter.createFileExtension(".doc", ".docx", ".pdf");
ISearchDocumentFilter orFilter = SearchDocumentFilter.createOr(filter3, filter4);
 
// Creating a filter that returns all found documents except of TXT documents
ISearchDocumentFilter filter5 = SearchDocumentFilter.createFileExtension(".txt");
ISearchDocumentFilter notFilter = SearchDocumentFilter.createNot(filter5);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
