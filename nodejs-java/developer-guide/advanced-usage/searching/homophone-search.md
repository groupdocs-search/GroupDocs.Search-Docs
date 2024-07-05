---
id: homophone-search
url: search/nodejs-java/homophone-search
title: Homophone search
weight: 9
description: "This article gives the knowledge which allows you to find not only the words specified in the search query, but also the homophones, words that are pronounced the same but differ in meaning using Java search API."
keywords: Homophone search
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Homophone search, or in other words phonic search, allows you to find not only the words specified in the search query, but also the homophones, words that are pronounced the same but differ in meaning.

To enable homophone search, you have to call the [setUseHomophoneSearch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setUseHomophoneSearch(boolean)) method of the [SearchOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions) class with the true value as an argument. By default, homophone search is disabled.

The default homophone dictionary contains homophones only for the English language. To manage the homophone dictionary, see the [Homophone dictionary]({{< ref "search/nodejs-java/developer-guide/advanced-usage/managing-dictionaries/homophone-dictionary.md" >}}) page in the [Managing dictionaries]({{< ref "search/nodejs-java/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The following example demonstrates the homophone search.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Creating a search options object
const options = new groupdocs.search.SearchOptions();
options.setUseHomophoneSearch(true); // Enabling homophone search

// Search for the word 'call'
// In addition to the word 'call', the word 'caul' will also be found
const query = 'call';
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
