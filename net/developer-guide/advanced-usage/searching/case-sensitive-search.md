---
id: case-sensitive-search
url: search/net/case-sensitive-search
title: Case sensitive search
weight: 2
description: "This article describes case-sensitive search, which allows you to find words considering uppercase and lowercase letters as distinct."
keywords: Case sensitive search 
productName: GroupDocs.Search for .NET
hideChildren: False
---
Case-sensitive search allows you to find words considering uppercase and lowercase letters as distinct. For example, in the context of case-sensitive search, the words 'Theory', 'theory', and 'THEORY' are all different.

Note that case-sensitive search cannot be combined with fuzzy search, synonym search, homophone search, word forms search, spelling correction, and keyboard layout correction (see [Search flow]({{< ref "search/net/developer-guide/advanced-usage/searching/search-flow.md" >}})). If case-sensitive search is enabled together with any of these options, the search does not throw an exception: it returns an empty result and raises the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index with a message describing the conflict. For details, see the [Search options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}}) page.

A word pattern (see [Wildcard search]({{< ref "search/net/developer-guide/advanced-usage/searching/wildcard-search.md" >}})) and a regular expression (see [Regular expression search]({{< ref "search/net/developer-guide/advanced-usage/searching/regular-expression-search.md" >}})) are matched with the words of the index without regard to the letter case. Since version 26.11, in a case-sensitive search such a part of a query finds the words in any letter case, and the [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) property of the result tells it by the line "Letter case ignored for query: " followed by the part of the query; the other parts of the query are case-sensitive. Before, such a part found only the occurrences of the words in lower case: for example, the pattern `Vend?(0~5)` found 'vendor' but not 'Vendor'. A word that such a part finds is given in the found terms ([FoundDocumentField.Terms](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/terms/)) in its form in lower case, in upper case or with the first letter in upper case, for example 'vendor', 'VENDOR' and 'Vendor', and the highlighting marks these forms as it marks the words of a case-sensitive search. A word written in another mix of the cases, for example 'VenDor', is found and counted, but it is given in lower case and is not highlighted. To find one form of a word, search for the word itself.

The following example demonstrates how to perform case-sensitive search with a query in text form.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
Index index = new Index(indexFolder); // Creating index in the specified folder
index.Add(documentsFolder); // Indexing documents from the specified folder
 
SearchOptions options = new SearchOptions();
options.UseCaseSensitiveSearch = true; // Enabling case sensitive search
 
string query = "Windows";
SearchResult result = index.Search(query, options); // Searching
```

The next example demonstrates how to perform case-sensitive search with a query in object form.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
Index index = new Index(indexFolder); // Creating index in the specified folder
index.Add(documentsFolder); // Indexing documents from the specified folder
 
SearchOptions options = new SearchOptions();
options.UseCaseSensitiveSearch = true; // Enabling case sensitive search
 
SearchQuery query = SearchQuery.CreateWordQuery("Windows"); // Creating search query in object form
 
SearchResult result = index.Search(query, options); // Searching
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
