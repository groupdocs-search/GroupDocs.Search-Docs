---
id: image-search-in-network
url: search/java/image-search-in-network
title: Image search in network
weight: 100
description: "This page contains information about reverse image search in the search network."
keywords: image search in network, image search in distributed index, reverse image search in network
productName: GroupDocs.Search for Java
hideChildren: False
---
To run a reverse image search, use the [searchFirst](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#searchFirst-com.groupdocs.search.common.SearchImage-com.groupdocs.search.options.ImageSearchOptions-) method of the [Searcher](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/) class. This method returns the first portion of results.

To continue the search, the [searchNext](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#searchNext-com.groupdocs.search.scaling.results.NetworkImageSearchToken-) method is used, returning the second and subsequent portions of results.

The search image object and search options object are the same as those used for single index searches.

The following code example demonstrates performing a reverse image search on the search network.

```java
System.out.println("First image search");

Searcher searcher = node.getSearcher();
ImageSearchOptions options = new ImageSearchOptions();
options.setHashDifferences(hashDifferences);
int total = 0;

NetworkImageSearchResult result = searcher.searchFirst(searchImage, options);
System.out.println("Images found (shard " + result.getShardIndex() + "): " + result.getImageCount());
total += result.getImageCount();

while (result.getNetworkImageSearchToken() != null) {
    System.out.println();
    System.out.println("Next image search");

    result = searcher.searchNext(result.getNetworkImageSearchToken());
    System.out.println("Images found (shard " + result.getShardIndex() + "): " + result.getImageCount());
    total += result.getImageCount();
}

System.out.println();
System.out.println("Total images found (diffs = " + hashDifferences + "): " + total);
```

More information about reverse image search in the [article](https://docs.groupdocs.com/search/java/reverse-image-search/).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
