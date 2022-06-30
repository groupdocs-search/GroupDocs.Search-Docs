---
id: search-reports
url: search/java/search-reports
title: Search reports
weight: 23
description: "This article shows that how to perform the operations on generated search reports."
keywords: search reports
productName: GroupDocs.Search for Java
hideChildren: False
---
Each time you perform a search in an index, a report is generated for that search. An array of search reports can be obtained by calling [getSearchReports](https://apireference.groupdocs.com/search/java/com.groupdocs.search/Index#getSearchReports()) method of the [Index](https://apireference.groupdocs.com/search/java/com.groupdocs.search/Index) class. Reports are stored in the index only while the index is loaded into RAM for use. If you reload the index, the reports will not be restored.

You can configure the maximum number of stored reports using the [setMaxSearchReportCount](https://apireference.groupdocs.com/search/java/com.groupdocs.search/IndexSettings#setMaxSearchReportCount(int)) method of the [IndexSettings](https://apireference.groupdocs.com/search/java/com.groupdocs.search/IndexSettings) class. The default value is 10. Learn more about index settings on the page [Search index settings]({{< ref "search/java/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}).

Each index search report contains the following information:

*   The start time of the search;
*   The end time of the search;
*   The search duration;
*   The number of documents found;
*   The total number of occurrences found;
*   The search query;
*   The search options.

The following example demonstrates how to get search reports from an index.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Searching in index
SearchResult result1 = index.search("Einstein");
SearchResult result2 = index.search("\"Theory of Relativity\"");
 
// Getting search reports
SearchReport[] reports = index.getSearchReports();
 
// Printing reports to the console
for (SearchReport report : reports) {
    System.out.println("Query: " + report.getTextQuery());
    System.out.println("Time: " + report.getStartTime());
    System.out.println("Duration: " + report.getSearchDuration());
    System.out.println("Documents: " + report.getDocumentCount());
    System.out.println("Occurrences: " + report.getOccurrenceCount());
    System.out.println();
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
