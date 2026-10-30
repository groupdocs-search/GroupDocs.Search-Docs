---
id: indexing-reports
url: search/python-net/indexing-reports
title: Indexing reports
weight: 17
description: "This article explains how to retrieve indexing reports with information on indexing and updating operations from an index."
keywords: indexing reports, IndexingReport, get_indexing_reports, max_indexing_report_count, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Indexing reports are created for indexing and updating operations. Indexing reports can be retrieved from the index using the [get_indexing_reports](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_indexing_reports/) method. Reports are stored in the index only while the index is loaded into RAM for use. If you reload the index, the reports will not be restored.

You can configure the maximum number of stored reports using the [max_indexing_report_count](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/max_indexing_report_count/) property of the [IndexSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/) class. The default value is 5. Learn more about index settings on the page [Search index settings]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}).

Each indexing report ([IndexingReport](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/)) contains the following information:

*   The total number of documents in the index ([total_documents_in_index](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/total_documents_in_index/));
*   The total number of terms in the index ([total_term_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/total_term_count/));
*   The total length of indexed documents in MB ([indexed_documents_size](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/indexed_documents_size/));
*   The number of index segments ([segment_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/segment_count/));
*   The total size of the index on disk in bytes ([total_index_size](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/total_index_size/));
*   The indexing start time, a `datetime.datetime` ([start_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/start_time/));
*   The indexing end time ([end_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/end_time/));
*   The indexing duration, a `datetime.timedelta` ([indexing_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/indexing_time/));
*   The list of errors ([errors](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/errors/));
*   The list of indexed documents ([indexed_documents](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/indexed_documents/));
*   The list of updated documents ([updated_documents](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/updated_documents/));
*   The list of removed documents ([removed_documents](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexingreport/removed_documents/)).

The following example demonstrates how to get indexing reports from an index.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

documents_folder1 = "./documents"
documents_folder2 = "./documents2"

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents
index.add(documents_folder1)
index.add(documents_folder2)

# Getting indexing reports
reports = index.get_indexing_reports()

# Printing information from reports to the console
for report in reports:
    print("Time: " + str(report.start_time))
    print("Duration: " + str(report.indexing_time))
    print("Documents total: " + str(report.total_documents_in_index))
    print("Terms total: " + str(report.total_term_count))
    print("Indexed documents size (MB): " + str(report.indexed_documents_size))
    print("Index size (MB): " + str(report.total_index_size / 1024.0 / 1024.0))
    print("Segments: " + str(report.segment_count))
    print("Indexed documents: " + str(len(report.indexed_documents)))
    for error in report.errors:
        print("Error: " + error)
    print()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-reports/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-reports/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "documents2" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents2/Curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-reports/documents2/Curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-reports.txt" >}}  
```text
Time: 2026-10-29 19:05:39.198203+02:00
Duration: 0:00:00.135150
Documents total: 2
Terms total: 86
Indexed documents size (MB): 0.000308990478515625
Index size (MB): 2.0021800994873047
Segments: 1
Indexed documents: 2

Time: 2026-10-29 19:05:39.333857+02:00
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-reports/indexing_reports/indexing-reports.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
