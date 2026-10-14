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
