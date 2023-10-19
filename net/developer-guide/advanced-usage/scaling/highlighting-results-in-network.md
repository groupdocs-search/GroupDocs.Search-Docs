---
id: highlighting-results-in-network
url: search/net/highlighting-results-in-network
title: Highlighting results in network
weight: 110
description: "This page contains information about highlighting search results in the search network."
keywords: highlighting search results in network, highlighting results in distributed index, highlighting results in search network
productName: GroupDocs.Search for .NET
hideChildren: False
---
Search results in the search network can be highlighted in the text of the found document using the [Highlight](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/highlight/) method of the [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/) class.

The set of available classes for highlighting text, as well as the highlighting options, are identical to those used for highlighting search results in a single index. Please see also the [Highlighting search results](https://docs.groupdocs.com/search/net/highlighting-search-results/) page.

The following code example demonstrates highlighting search results on the search network.

**C#**

```csharp
Searcher searcher = node.Searcher;

FragmentHighlighter highlighter = new FragmentHighlighter(OutputFormat.PlainText);

HighlightOptions options = new HighlightOptions();
options.TermsAfter = 5;
options.TermsBefore = 5;
options.TermsTotal = 15;
searcher.Highlight(document, highlighter, options);

FragmentContainer[] result = highlighter.GetResult();
Console.WriteLine();
for (int i = 0; i < result.Length; i++)
{
    FragmentContainer container = result[i];
    if (container.Count == 0) continue;

    string[] fragments = container.GetFragments();
    Console.WriteLine(container.FieldName);
    int count = Math.Min(fragments.Length, maxFragments);
    for (int j = 0; j < count; j++)
    {
        Console.WriteLine("\t" + fragments[j]);
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
