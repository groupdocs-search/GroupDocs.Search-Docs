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
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search with text query
const query1 = 'theory AND relativity';
const result1 = index.search(query1);

// Search with object query
const wordQuery1 = groupdocs.search.SearchQuery.createWordQuery('theory');
const wordQuery2 = groupdocs.search.SearchQuery.createWordQuery('relativity');
const andQuery = groupdocs.search.SearchQuery.createAndQuery(wordQuery1, wordQuery2);
const result2 = index.search(andQuery);
```

## Operator OR

The OR operator allows you to find all the documents that are found for at least one nested search query. The example demonstrates the use of the OR operator in text and object form queries. The queries search for documents in the text of which there is at least one of the words "Einstein" and "relativity".

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search with text query
const query1 = 'Einstein OR relativity';
const result1 = index.search(query1);

// Search with object query
const wordQuery1 = groupdocs.search.SearchQuery.createWordQuery('Einstein');
const wordQuery2 = groupdocs.search.SearchQuery.createWordQuery('relativity');
const orQuery = groupdocs.search.SearchQuery.createOrQuery(wordQuery1, wordQuery2);
const result2 = index.search(orQuery);
```

## Operator NOT

The NOT operator allows you to invert the result of a nested search query and find all documents in which for the nested search query are found no occurrences. The example demonstrates the use of the NOT operator in text and object form queries. The queries search for documents in the text of which the word "relativity" is presented but the word "Einstein" is not.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search with text query
const query1 = 'relativity AND NOT Einstein';
const result1 = index.search(query1);

// Search with object query
const wordQuery1 = groupdocs.search.SearchQuery.createWordQuery('relativity');
const wordQuery2 = groupdocs.search.SearchQuery.createWordQuery('Einstein');
const notQuery = groupdocs.search.SearchQuery.createNotQuery(wordQuery2);
const andQuery = groupdocs.search.SearchQuery.createAndQuery(wordQuery1, notQuery);
const result2 = index.search(andQuery);
```

## Complex queries

Boolean search operators can be combined using parentheses. The example below shows how to use parentheses to construct complex boolean search queries. In the example the query is presented in text and object form and searches for documents containing the words "theory" and "relativity" and not containing the words "Einstein" and "Albert".

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search with text query
const query1 = '(sportsman AND favourable) AND NOT (Kynynmound OR Murray)';
const result1 = index.search(query1);

// Search with object query
const word1Query = groupdocs.search.SearchQuery.createWordQuery('sportsman');
const word2Query = groupdocs.search.SearchQuery.createWordQuery('favourable');
const andQuery = groupdocs.search.SearchQuery.createAndQuery(word1Query, word2Query);

const word3Query = groupdocs.search.SearchQuery.createWordQuery('Kynynmound');
const word4Query = groupdocs.search.SearchQuery.createWordQuery('Murray');
const orQuery = groupdocs.search.SearchQuery.createOrQuery(word3Query, word4Query);
const notQuery = groupdocs.search.SearchQuery.createNotQuery(orQuery);

const rootQuery = groupdocs.search.SearchQuery.createAndQuery(andQuery, notQuery);
const result2 = index.search(rootQuery);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
