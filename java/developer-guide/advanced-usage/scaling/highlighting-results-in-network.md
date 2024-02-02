---
id: highlighting-results-in-network
url: search/java/highlighting-results-in-network
title: Highlighting results in network
weight: 110
description: "This page contains information about highlighting search results in the search network."
keywords: highlighting search results in network, highlighting results in distributed index, highlighting results in search network
productName: GroupDocs.Search for Java
hideChildren: False
---
Search results in the search network can be highlighted in the text of the found document using the [highlight](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#highlight-com.groupdocs.search.scaling.results.NetworkFoundDocument-com.groupdocs.search.highlighters.Highlighter-com.groupdocs.search.options.HighlightOptions-) method of the [Searcher](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/) class.

The set of available classes for highlighting text, as well as the highlighting options, are identical to those used for highlighting search results in a single index. Please see also the [Highlighting search results](https://docs.groupdocs.com/search/java/highlighting-search-results/) page.

The following code example demonstrates highlighting search results on the search network.

```java
Searcher searcher = node.getSearcher();

FragmentHighlighter highlighter = new FragmentHighlighter(OutputFormat.PlainText);

HighlightOptions options = new HighlightOptions();
options.setTermsAfter(5);
options.setTermsBefore(5);
options.setTermsTotal(15);
searcher.highlight(document, highlighter, options);

FragmentContainer[] result = highlighter.getResult();
System.out.println();
for (FragmentContainer container : result) {
    if (container.getCount() == 0) continue;

    String[] fragments = container.getFragments();
    System.out.println(container.getFieldName());
    int count = Math.min(fragments.length, maxFragments);
    for (int j = 0; j < count; j++) {
        System.out.println("\t" + fragments[j]);
    }
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
