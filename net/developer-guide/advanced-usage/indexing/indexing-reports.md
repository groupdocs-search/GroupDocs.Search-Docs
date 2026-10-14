---
id: indexing-reports
url: search/net/indexing-reports
title: Indexing reports
weight: 17
description: "This article explains how to retrieve indexing reports with information on indexing and updating operations from an index."
keywords: indexing reports, IndexingReport, GetIndexingReports, MaxIndexingReportCount
productName: GroupDocs.Search for .NET
hideChildren: False
---
Indexing reports are created for indexing and updating operations. Indexing reports can be retrieved from the index using the [GetIndexingReports](https://reference.groupdocs.com/search/net/groupdocs.search/index/getindexingreports/) method. Reports are stored in the index only while the index is loaded into RAM for use. If you reload the index, the reports will not be restored.

You can configure the maximum number of stored reports using the [MaxIndexingReportCount](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/maxindexingreportcount/) property of the [IndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/) class. The default value is 5. Learn more about index settings on the page [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}).

Each indexing report ([IndexingReport](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/)) contains the following information:

*   The total number of documents in the index ([TotalDocumentsInIndex](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/totaldocumentsinindex/));
*   The total number of terms in the index ([TotalTermCount](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/totaltermcount/));
*   The total length of indexed documents in MB ([IndexedDocumentsSize](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/indexeddocumentssize/));
*   The number of index segments ([SegmentCount](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/segmentcount/));
*   The total size of the index on disk in bytes ([TotalIndexSize](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/totalindexsize/));
*   The indexing start time ([StartTime](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/starttime/));
*   The indexing end time ([EndTime](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/endtime/));
*   The indexing duration ([IndexingTime](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/indexingtime/));
*   The list of errors ([Errors](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/errors/));
*   The list of indexed documents ([IndexedDocuments](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/indexeddocuments/));
*   The list of updated documents ([UpdatedDocuments](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/updateddocuments/));
*   The list of removed documents ([RemovedDocuments](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/removeddocuments/)).

The following example demonstrates how to get indexing reports from an index.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder1 = @"c:\MyDocuments1\";
string documentsFolder2 = @"c:\MyDocuments2\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents
index.Add(documentsFolder1);
index.Add(documentsFolder2);
 
// Getting indexing reports
IndexingReport[] reports = index.GetIndexingReports();
 
// Printing information from reports to the console
foreach (IndexingReport report in reports)
{
    Console.WriteLine("Time: " + report.StartTime);
    Console.WriteLine("Duration: " + report.IndexingTime);
    Console.WriteLine("Documents total: " + report.TotalDocumentsInIndex);
    Console.WriteLine("Terms total: " + report.TotalTermCount);
    Console.WriteLine("Indexed documents size (MB): " + report.IndexedDocumentsSize);
    Console.WriteLine("Index size (MB): " + (report.TotalIndexSize / 1024.0 / 1024.0));
    Console.WriteLine("Segments: " + report.SegmentCount);
    Console.WriteLine("Indexed documents: " + report.IndexedDocuments.Length);
    foreach (string error in report.Errors)
    {
        Console.WriteLine("Error: " + error);
    }
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
