---
id: synonym-search
url: search/nodejs-java/synonym-search
title: Synonym search
weight: 27
description: "This article shows that how synonym search allows you to find not only the words specified in the search query, but also the synonyms, words that means the same."
keywords: Synonym search
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Synonym search allows you to find not only the words specified in the search query, but also the synonyms, words that means the same.

To enable synonym search, you must call the [setUseSynonymSearch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setUseSynonymSearch(boolean)) method of the [SearchOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions) class with the true value as an argument. By default, synonym search is disabled.

The default synonym dictionary contains synonyms only for the English language. To manage the synonym dictionary, see the [Synonym dictionary]({{< ref "search/nodejs-java/developer-guide/advanced-usage/managing-dictionaries/synonym-dictionary.md" >}}) page in the [Managing dictionaries]({{< ref "search/nodejs-java/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The following example demonstrates the synonym search.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Creating a search options object
const options = new groupdocs.search.SearchOptions();
options.setUseSynonymSearch(true); // Enabling synonym search

// Search for the word 'improve'
// In addition to the word 'improve', the words 'better' will also be found
const query = 'improve';
const result = index.search(query, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
