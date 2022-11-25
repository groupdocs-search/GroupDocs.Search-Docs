---
id: search-for-different-word-forms
url: search/java/search-for-different-word-forms
title: Search for different word forms
weight: 20
description: "This article shows that how to allow you to search for nouns in the singular or plural, adjectives in the degree of comparison, forms of regular and irregular verbs, etc."
keywords: Search for different word forms
productName: GroupDocs.Search for Java
hideChildren: False
---
The search for different word forms allows you to search for nouns in the singular or plural, adjectives in the degree of comparison, forms of regular and irregular verbs, etc.

Search for different word forms is enabled if the [setUseWordFormsSearch](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/SearchOptions#setUseWordFormsSearch(boolean)) method of the [SearchOptions](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/SearchOptions) class is called with the true value as an argument. By default, the search for different word forms is disabled.

To generate various forms of words, a class that implements the [IWordFormsProvider](https://reference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/IWordFormsProvider) interface is used. The default class is [EnglishWordFormsProvider](https://reference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/EnglishWordFormsProvider), which supports English-only word forms. To add support for word forms in other languages, see the [Word forms provider]({{< ref "search/java/developer-guide/advanced-usage/managing-dictionaries/word-forms-provider.md" >}}) page.

The following example demonstrates how to perform search for different word forms in an index.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Creating a search options instance
SearchOptions options = new SearchOptions();
options.setUseWordFormsSearch(true); // Enabling search for word forms
 
// Searching in the index
SearchResult result = index.search("relative", options);
 
// The following words can be found:
// relative
// relatives
// relatively
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
