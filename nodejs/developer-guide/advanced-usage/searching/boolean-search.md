---
id: boolean-search
url: search/nodejs-java/boolean-search
title: Boolean search
weight: 1
description: "This article gives the knowledge of the boolean search definition which includes all boolean operators used for boolean search, and boolean query examples using Java."
keywords: boolean search definition, boolean search, boolean query, boolean query examples 
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page contains the boolean search definition, a description of all boolean operators used for boolean search, and boolean query examples.

## Boolean search terms

Boolean search is a type of search that allows you to combine queries with boolean operators AND, OR, NOT to create a boolean query and further obtain more relevant results. For example, a boolean query might be "Albert AND Einstein". This will limit the search results to only documents that contain both words.

## Operator AND

The AND operator allows you to find only those documents that are found for each nested search query separately. The following example demonstrates the use of the AND operator in text and object form queries. The queries search for documents in the text of which there are both words "theory" and "relativity".

```javascript
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search with text query
SearchResult result1 = index.search("theory AND relativity");
 
// Search with object query
SearchQuery wordQuery1 = SearchQuery.createWordQuery("theory");
SearchQuery wordQuery2 = SearchQuery.createWordQuery("relativity");
SearchQuery andQuery = SearchQuery.createAndQuery(wordQuery1, wordQuery2);
SearchResult result2 = index.search(andQuery);
```

## Operator OR

The OR operator allows you to find all the documents that are found for at least one nested search query. The example demonstrates the use of the OR operator in text and object form queries. The queries search for documents in the text of which there is at least one of the words "Einstein" and "relativity".

```javascript
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search with text query
SearchResult result1 = index.search("Einstein OR relativity");
 
// Search with object query
SearchQuery wordQuery1 = SearchQuery.createWordQuery("Einstein");
SearchQuery wordQuery2 = SearchQuery.createWordQuery("relativity");
SearchQuery orQuery = SearchQuery.createOrQuery(wordQuery1, wordQuery2);
SearchResult result2 = index.search(orQuery);
```

## Operator NOT

The NOT operator allows you to invert the result of a nested search query and find all documents in which for the nested search query are found no occurrences. The example demonstrates the use of the NOT operator in text and object form queries. The queries search for documents in the text of which the word "relativity" is presented but the word "Einstein" is not.

```javascript
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search with text query
SearchResult result1 = index.search("relativity AND NOT Einstein");
 
// Search with object query
SearchQuery wordQuery1 = SearchQuery.createWordQuery("relativity");
SearchQuery wordQuery2 = SearchQuery.createWordQuery("Einstein");
SearchQuery notQuery = SearchQuery.createNotQuery(wordQuery2);
SearchQuery andQuery = SearchQuery.createAndQuery(wordQuery1, notQuery);
SearchResult result2 = index.search(andQuery);
```

## Complex queries

Boolean search operators can be combined using parentheses. The example below shows how to use parentheses to construct complex boolean search queries. In the example the query is presented in text and object form and searches for documents containing the words "theory" and "relativity" and not containing the words "Einstein" and "Albert".

```javascript
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search with text query
SearchResult result1 = index.search("(theory AND relativity) AND NOT (Einstein OR Albert)");
 
// Search with object query
SearchQuery theoryWordQuery = SearchQuery.createWordQuery("theory");
SearchQuery relativityWordQuery = SearchQuery.createWordQuery("relativity");
SearchQuery andQuery = SearchQuery.createAndQuery(theoryWordQuery, relativityWordQuery);
 
SearchQuery einsteinWordQuery = SearchQuery.createWordQuery("Einstein");
SearchQuery albertWordQuery = SearchQuery.createWordQuery("Albert");
SearchQuery orQuery = SearchQuery.createOrQuery(einsteinWordQuery, albertWordQuery);
SearchQuery notQuery = SearchQuery.createNotQuery(orQuery);
 
SearchQuery rootQuery = SearchQuery.createAndQuery(andQuery, notQuery);
SearchResult result2 = index.search(rootQuery);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
