---
id: indexing-reports
url: search/nodejs-java/indexing-reports
title: Indexing reports
weight: 14
description: ""
keywords: 
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Indexing reports are created for indexing and updating operations. Indexing reports can be retrieved from the index using the [getIndexingReports](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#getIndexingReports()) method. Reports are stored in the index only while the index is loaded into RAM for use. If you reload the index, the reports will not be restored.

You can configure the maximum number of stored reports using the [setMaxIndexingReportCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setMaxIndexingReportCount(int)) method of the [IndexSettings](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings) class. The default value is 5. Learn more about index settings on the page [Search index settings]({{< ref "search/nodejs-java/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}).

Each index report contains the following information:

*   The total number of documents in the index;
*   The total number of words in the index;
*   The total length of indexed documents in MB;
*   Number of index segments;
*   The total size of the index on disk in bytes;
*   The indexing start time;
*   The indexing end time;
*   The indexing duration;
*   List of errors;
*   List of indexed documents;
*   List of updated documents;
*   List of removed documents.

The following example demonstrates how to get indexing reports from an index.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder1 = 'c:/MyDocuments1/';
const documentsFolder2 = 'c:/MyDocuments2/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents
index.add(documentsFolder1);
index.add(documentsFolder2);

// Getting indexing reports
const reports = index.getIndexingReports();

// Printing information from reports to the console
for (const report of reports) {
  console.log('Time: ' + report.getStartTime());
  console.log('Duration: ' + report.getIndexingTime());
  console.log('Documents total: ' + report.getTotalDocumentsInIndex());
  console.log('Terms total: ' + report.getTotalTermCount());
  console.log('Indexed documents size (MB): ' + report.getIndexedDocumentsSize());
  console.log('Index size (MB): ' + report.getTotalIndexSize() / 1024.0 / 1024.0);
  console.log();
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
