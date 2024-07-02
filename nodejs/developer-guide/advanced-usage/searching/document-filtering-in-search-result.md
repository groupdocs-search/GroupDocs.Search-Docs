---
id: document-filtering-in-search-result
url: search/nodejs-java/document-filtering-in-search-result
title: Document filtering in search result
weight: 4
description: "This article gives the knowledge that how the document filters used during the search using Java search API."
keywords: Document filtering, Document filtering in search result
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page contains a description of the document filters used during the search.

## Setting a filter

To specify which of the documents found should be returned as a result of the search, the [setSearchDocumentFilter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setSearchDocumentFilter(com.groupdocs.search.options.ISearchDocumentFilter)) method of the [SearchOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions) class is used. If the document found does not match a filter passed to this method as an argument, the document will not be returned. The default value is null, which means that all documents found will be returned. The following example shows how to set a document filter for searching.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Creating a search options object
const options = new groupdocs.search.SearchOptions();
options.setSearchDocumentFilter(groupdocs.search.SearchDocumentFilter.createFileExtension('.txt')); // Setting a document filter

// Search in the index
// Only text documents will be returned as the result of the search
const query = 'education';
const result = index.search(query, options);
```

## File path filters

The first supported type of search document filters allows you to set a regular expression for getting those documents whose full paths match the specified pattern. This type of filters uses the **java.util.regex.Pattern** class to compare with a pattern.

```javascript
// The filter returns only files that contain the word 'Lorem' in their paths, not case sensitive
const filter = groupdocs.search.SearchDocumentFilter.createFilePathRegularExpression(
  'Lorem',
  java.getStaticFieldValue('java.util.regex.Pattern', 'CASE_INSENSITIVE'),
);
```

## File extension filter

The next supported type of search document filters allows you to specify a list of valid file extensions for indexing.

```javascript
// This filter returns only PDF and DOCX documents
const filter = groupdocs.search.SearchDocumentFilter.createFileExtension('.pdf', '.docx');
```

## Attribute filter

The next supported type of search document filters allows you to search only those documents with which the specified text attribute is associated. You can learn more about attributes on the [Document attributes]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) page.

```javascript
// This filter returns only documents that have attribute "main"
const filter = groupdocs.search.SearchDocumentFilter.createAttribute('main');
```

## Combining filters

Search document filters can be combined using composite filters AND, OR, NOT. The following example shows how to combine search document filters.

```javascript
// Creating an AND composite filter that returns all FB2 and EPUB documents that have the word 'Einstein' in their full paths
const filter1 = groupdocs.search.SearchDocumentFilter.createFilePathRegularExpression(
  'Einstein',
  java.getStaticFieldValue('java.util.regex.Pattern', 'CASE_INSENSITIVE'),
);
const filter2 = groupdocs.search.SearchDocumentFilter.createFileExtension('.fb2', '.epub');
const andFilter = groupdocs.search.SearchDocumentFilter.createAnd(filter1, filter2);

// Creating an OR composite filter that returns all DOC, DOCX, PDF and all documents that have the word Einstein in their full paths
const filter3 = groupdocs.search.SearchDocumentFilter.createFilePathRegularExpression(
  'Einstein',
  java.getStaticFieldValue('java.util.regex.Pattern', 'CASE_INSENSITIVE'),
);
const filter4 = groupdocs.search.SearchDocumentFilter.createFileExtension('.doc', '.docx', '.pdf');
const orFilter = groupdocs.search.SearchDocumentFilter.createOr(filter3, filter4);

// Creating a filter that returns all found documents except of TXT documents
const filter5 = groupdocs.search.SearchDocumentFilter.createFileExtension('.txt');
const notFilter = groupdocs.search.SearchDocumentFilter.createNot(filter5);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
