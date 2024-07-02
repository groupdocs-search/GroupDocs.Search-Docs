---
id: wildcard-search
url: search/nodejs-java/wildcard-search
title: Wildcard search
weight: 29
description: "This article shows the operations of wildcard search which allows you to search for words with unknown letters or ranges of letters."
keywords: Wildcard search
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Wildcard search allows you to search for words with unknown letters or ranges of letters.

In text form of a search query, there are 2 forms of wildcard characters:

*   ? for a single character;
*   ?(*n*~*m*) for a group of characters, where *n* and *m* are numbers from 0 to 255, and *n* <= *m*.

Wildcard search is similar to [regular expression search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/regular-expression-search.md" >}}), but it works significantly faster when groups of wildcard characters are less and closer to the end of a search query.

It is important to know that wildcard search is flexible enough to use for prefix queries, since prefix query is a special case of a wildcard query.

Examples of wildcard search with queries in text form are presented below.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search in the index
const query1 = 'm???is';
const result1 = index.search(query1); // Search for 'mauris', 'mollis', 'mattis', 'magnis', etc.
const query2 = 'pri?(1~7)';
const result2 = index.search(query2); // Search for 'private', 'principles', 'principle', etc.
```

To build a query for the wildcard search in object form, use the [WordPattern](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/WordPattern) class. This class contains methods for adding known parts of a word and wildcards to a template. An example of constructing a query in object form is presented below.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search with pattern "m???is"
// Search for 'mauris', 'mollis', 'mattis', 'magnis', etc.
const pattern1 = new groupdocs.search.WordPattern();
pattern1.appendString('m');
pattern1.appendOneCharacterWildcard();
pattern1.appendOneCharacterWildcard();
pattern1.appendOneCharacterWildcard();
pattern1.appendString('is');
const query1 = groupdocs.search.SearchQuery.createWordPatternQuery(pattern1);
const result1 = index.search(query1);

// Search with pattern "pri?(1~7)"
// Search for 'private', 'principles', 'principle', etc.
const pattern2 = new groupdocs.search.WordPattern();
pattern2.appendString('pri');
pattern2.appendWildcard(1, 7);
const query2 = groupdocs.search.SearchQuery.createWordPatternQuery(pattern2);
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
