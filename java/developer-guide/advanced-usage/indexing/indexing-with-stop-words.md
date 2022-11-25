---
id: indexing-with-stop-words
url: search/java/indexing-with-stop-words
title: Indexing with stop words
weight: 15
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
Stop words are frequently used words that do not carry a semantic meaning and can be removed from an index to reduce its size.

You can enable or disable the use of stop words by calling the [setUseStopWords](https://reference.groupdocs.com/search/java/com.groupdocs.search/IndexSettings#setUseStopWords(boolean)) method of the [IndexSettings](https://reference.groupdocs.com/search/java/com.groupdocs.search/IndexSettings) class. The default value is true, meaning that stop words are filtered during indexing and not added to the index.

A list of stop words to use during indexing can be specified in the stop word dictionary. By default, the stop word dictionary is filled with the most widely used pronouns and prepositions of English and Russian. The list of stop words used can be easily replaced or supplemented and it is saved when the index is reloaded. For information on managing the stop word dictionary, see the [Stop word dictionary]({{< ref "search/java/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary.md" >}}) page in the [Managing dictionaries]({{< ref "search/java/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

If you need to keep all text information extracted from documents, and you are not afraid of a significant increase in the size of the index, then an example of indexing without stop words can be found below.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index settings with disabled using of stop words
IndexSettings settings = new IndexSettings();
settings.setUseStopWords(false);
 
// Creating an index in the specified folder
Index index = new Index(indexFolder, settings);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Searching in the index
// Now in the index it is possible to search for the stop word 'on'
SearchResult result = index.search("on");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
