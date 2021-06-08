---
id: case-sensitive-search
url: search/java/case-sensitive-search
title: Case sensitive search
weight: 2
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
Case-sensitive search allows you to find words considering uppercase and lowercase letters as distinct. For example, words in context of case-sensitive search 'Theory', 'theory', and 'THEORY' are all different.

Note that case-sensitive search is not compatible with other types of search (see [Search flow]({{< ref "search/java/developer-guide/advanced-usage/searching/search-flow.md" >}})).

The following example demonstrates how to perform case-sensitive search with a query in text form.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
SearchOptions options = new SearchOptions();
options.setUseCaseSensitiveSearch(true); // Enabling case sensitive search
 
// Searching in the index
String query = "Windows";
SearchResult result = index.search(query, options);
```

The next example demonstrates how to perform case-sensitive search with a query in object form.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
SearchOptions options = new SearchOptions();
options.setUseCaseSensitiveSearch(true); // Enabling case sensitive search
 
// Creating search query in object form
SearchQuery query = SearchQuery.createWordQuery("Windows");
 
// Searching in the index
SearchResult result = index.search(query, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
