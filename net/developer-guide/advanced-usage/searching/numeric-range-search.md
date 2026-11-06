---
id: numeric-range-search
url: search/net/numeric-range-search
title: Numeric range search
weight: 12
description: "This article describes numeric range search, which allows you to search documents for integer numbers in the range from 0 to 9223372036854775807 (Int64.MaxValue)."
keywords: numeric range search
productName: GroupDocs.Search for .NET
hideChildren: False
---
Numeric range search or numerical search allows you to search documents for integer numbers in the range from 0 to 9223372036854775807 (Int64.MaxValue). Please note that a number in the text must not be separated by spaces; otherwise, it is treated as several numbers. A search query of this type is specified as follows:

number ~~ number

The example below demonstrates the numeric range search in text and object forms.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Search with text query
SearchResult result1 = index.Search("500 ~~ 600");
 
// Search with object query
SearchQuery query2 = SearchQuery.CreateNumericRangeQuery(500, 600);
SearchResult result2 = index.Search(query2);
```

Since version 26.10, numeric range search uses as many threads as there are processor cores by default. Since version 26.11, the searches of an index that run at the same time share these threads (see the [Search thread safety]({{< ref "search/net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}}) page), and the order of the found numbers in the search result does not depend on the number of search threads (the [SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/) index setting); before, it could differ from run to run.

Also since version 26.11:

*   The numbers of a range that are found in the index are kept for the next searches with the same query until the index is changed, so a repeated search is faster.
*   A search is canceled (see the [Search options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md" >}}) page) also while it looks for the numbers of the range among the words of the index.
*   The numbers of a query in text form are read in the same way in any culture of the current thread: before, a negative number, as in `-5~~10`, failed in some cultures.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
