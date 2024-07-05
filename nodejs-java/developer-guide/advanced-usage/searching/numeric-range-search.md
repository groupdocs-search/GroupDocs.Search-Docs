---
id: numeric-range-search
url: search/nodejs-java/numeric-range-search
title: Numeric range search
weight: 12
description: "This article gives the knowledge about numeric range search which allows you to search in documents any integer numbers in the range from 0 to 9223372036854775807 (Int64.MaxValue) using Java search API."
keywords: numeric range search
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Numeric range search or numerical search allows you to search in documents any integer numbers in the range from 0 to 9223372036854775807. Please note that the number in the text must not be separated by spaces, otherwise it will already be several numbers. A search query of this type is specified as follows:

number ~~ number

The example below demonstrates the numeric range search in text and object forms.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search with text query
const query1 = '400 ~~ 4000';
const result1 = index.search(query1);

// Search with object query
const query2 = groupdocs.search.SearchQuery.createNumericRangeQuery(400, 4000);
const result2 = index.search(query2);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
