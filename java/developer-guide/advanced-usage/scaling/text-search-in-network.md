---
id: text-search-in-network
url: search/java/text-search-in-network
title: Text search in network
weight: 90
description: "This page contains information about full-text searching in the search network."
keywords: full-text search in network, text search in distributed index
productName: GroupDocs.Search for Java
hideChildren: False
---
To run a text search, use the [searchFirst](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#searchFirst-java.lang.String-com.groupdocs.search.options.SearchOptions-) method of the [Searcher](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/) class. This method returns the first portion of results.

To continue the search, the [searchNext](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#searchNext-com.groupdocs.search.scaling.results.NetworkSearchToken-) method is used, returning the second and subsequent portions of results.

Text and object forms of the search query are supported when appropriate overloads of the SearchFirst method are used. The options for a network search are the same as those for a single index search.

The following code example demonstrates performing a full-text search on the search network.

```java
Searcher searcher = node.getSearcher();

SearchOptions options = new SearchOptions();
options.setChunkSearch(true);
options.setUseSynonymSearch(useSynonymSearch);
int totalOccurrences = 0;
ArrayList<NetworkFoundDocument> documents = new ArrayList<NetworkFoundDocument>();

NetworkSearchResult result;
if (isObjectForm) {
    SearchQuery query = SearchQuery.createWordQuery(word);
    System.out.println();
    System.out.println("First time search in object form for: " + query);
    result = searcher.searchFirst(query, options);
} else {
    String query = word;
    System.out.println();
    System.out.println("First time search in text form for: " + query);
    result = searcher.searchFirst(query, options);
}

addDocsFromResult(documents, result);
totalOccurrences += result.getOccurrenceCount();
traceResult(result);

while (result.getNextChunkSearchToken() != null) {
    System.out.println();
    System.out.println("Next time search for: " + word);

    result = searcher.searchNext(result.getNextChunkSearchToken());

    addDocsFromResult(documents, result);
    totalOccurrences += result.getOccurrenceCount();
    traceResult(result);
}

System.out.println();
System.out.println("Total occurrences: " + totalOccurrences);
```

More information about search queries can be found in the article [Queries in text and object form](https://docs.groupdocs.com/search/java/queries-in-text-and-object-form/).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
