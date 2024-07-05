---
id: case-sensitive-search
url: search/nodejs-java/case-sensitive-search
title: Case sensitive search
weight: 2
description: "This article gives the knowledge of the case sensitive search which allows you to find words considering uppercase and lowercase letters as distinct using Java."
keywords: Case sensitive search 
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Case-sensitive search allows you to find words considering uppercase and lowercase letters as distinct. For example, words in context of case-sensitive search 'Theory', 'theory', and 'THEORY' are all different.

Note that case-sensitive search is not compatible with other types of search (see [Search flow]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/search-flow.md" >}})).

The following example demonstrates how to perform case-sensitive search with a query in text form.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

const index = new groupdocs.search.Index(indexFolder); // Creating index in the specified folder
index.add(documentsFolder); // Indexing documents from the specified folder

const options = new groupdocs.search.SearchOptions();
options.setUseCaseSensitiveSearch(true); // Enabling case sensitive search

const query = 'Advantages';
const result = index.search(query, options); // Searching
```

The next example demonstrates how to perform case-sensitive search with a query in object form.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

const index = new groupdocs.search.Index(indexFolder); // Creating index in the specified folder
index.add(documentsFolder); // Indexing documents from the specified folder

const options = new groupdocs.search.SearchOptions();
options.setUseCaseSensitiveSearch(true); // Enabling case sensitive search

const query = groupdocs.search.SearchQuery.createWordQuery('Advantages'); // Creating search query in object form

const result = index.search(query, options); // Searching
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
