---
id: regular-expression-search
url: search/net/regular-expression-search
title: Regular expression search
weight: 17
description: "This article describes regular expression (RegEx) search queries, which are universal and very flexible but can be slow in large indexes."
keywords: Regular expression search
productName: GroupDocs.Search for .NET
hideChildren: False
---
Regular expression (RegEx) search queries are universal and very flexible, but at the same time, in large indexes, their performance can be low, because the regular expression is matched against the words of the index. Another limitation of regular expressions is that search with a pattern is done for each word separately, and not the entire text of a document field. Words are stored in the index in lower case, therefore regular expression search is always case-insensitive.

Queries of this type in text form always start with the caret character, and if the regular expression itself also starts with the caret character, then there will be two caret characters at the beginning of the query.

A regex search query in text form cannot be combined with other types of queries. See the [Query language specification]({{< ref "search/net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}) page. However, in object form, regular expressions can be combined with other queries. See the [Nesting search queries in object form]({{< ref "search/net/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form.md" >}}) page.

The following example demonstrates the regex search in text and object forms.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Search with a regular expression in text form
string query1 = "^^(.)\\1{1,}"; // The first caret character at the beginning indicates that this is a regular expression search query
SearchResult result1 = index.Search(query1); // Search for two or more identical characters at the beginning of a word
 
// Search with a regular expression in object form
SearchQuery query2 = SearchQuery.CreateRegexQuery("^(.)\\1{1,}"); // Search for two or more identical characters at the beginning of a word
SearchResult result2 = index.Search(query2);
```

In object form, a regular expression query can also be created with the [CreateRegexQuery](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/createregexquery/) method overload that takes a combination of [RegexOptions](https://learn.microsoft.com/dotnet/api/system.text.regularexpressions.regexoptions) values as the second argument. The options must contain the RegexOptions.IgnoreCase flag; otherwise, ArgumentException is thrown. The overload with one argument uses RegexOptions.IgnoreCase.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Creating a regular expression query with the specified options
RegexOptions regexOptions = RegexOptions.IgnoreCase | RegexOptions.CultureInvariant;
SearchQuery query = SearchQuery.CreateRegexQuery("^(.)\\1{1,}", regexOptions); // Search for two or more identical characters at the beginning of a word
SearchResult result = index.Search(query);
```

Since version 26.10, regular expression search uses as many threads as there are processor cores by default. Therefore, the order of the found words in the search result may differ from run to run. To get the same order every time, set the [SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/) index setting to NumberOfThreads.One.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
