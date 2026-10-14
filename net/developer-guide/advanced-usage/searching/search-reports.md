---
id: search-reports
url: search/net/search-reports
title: Search reports
weight: 23
description: "This article describes how to get the reports generated for search operations."
keywords: search reports
productName: GroupDocs.Search for .NET
hideChildren: False
---
Each time you perform a search in an index, a report is generated for that search. An array of search reports can be obtained by calling the [GetSearchReports](https://reference.groupdocs.com/search/net/groupdocs.search/index/getsearchreports/) method of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class. Reports are stored in the index only while the index is loaded into RAM for use. If you reload the index, the reports will not be restored.

You can configure the maximum number of stored reports using the [MaxSearchReportCount](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/maxsearchreportcount/) property of the [IndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/) class. The default value is 10. The value can be changed at any time. Learn more about index settings on the page [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}).

Each search report is represented by an instance of the [SearchReport](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/) class, which contains the following information:

*   [StartTime](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/starttime/) - the start time of the search;
*   [EndTime](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/endtime/) - the end time of the search;
*   [SearchDuration](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/searchduration/) - the search duration;
*   [DocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/documentcount/) - the number of documents found;
*   [OccurrenceCount](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/occurrencecount/) - the total number of occurrences found;
*   [TextQuery](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/textquery/) - the search query in text form, or null if the search was performed with a query in object form;
*   [ObjectQuery](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/objectquery/) - the search query in object form, or null if the search was performed with a query in text form;
*   [SearchOptions](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/searchoptions/) - the search options.

The [ToString](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchreport/tostring/) method returns the search query in text form for both kinds of queries.

The following example demonstrates how to get search reports from an index.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Searching in index
SearchResult result1 = index.Search("Einstein");
SearchResult result2 = index.Search("\"Theory of Relativity\"");
 
// Getting search reports
SearchReport[] reports = index.GetSearchReports();
 
// Printing reports to the console
foreach (SearchReport report in reports)
{
    Console.WriteLine("Query: " + report.ToString());
    Console.WriteLine("Time: " + report.StartTime);
    Console.WriteLine("Duration: " + report.SearchDuration);
    Console.WriteLine("Documents: " + report.DocumentCount);
    Console.WriteLine("Occurrences: " + report.OccurrenceCount);
    Console.WriteLine();
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
