---
id: search-reports
url: search/python-net/search-reports
title: Search reports
weight: 23
description: "This article describes how to get the reports generated for search operations."
keywords: search reports
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Each time you perform a search in an index, a report is generated for that search. A list of search reports can be obtained by calling the [get_search_reports](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_search_reports/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class. Reports are stored in the index only while the index is loaded into RAM for use. If you reload the index, the reports will not be restored.

You can configure the maximum number of stored reports using the [max_search_report_count](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/max_search_report_count/) property of the [IndexSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/) class. The default value is 10. The value can be changed at any time. Learn more about index settings on the page [Search index settings]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}).

Each search report is represented by an instance of the [SearchReport](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/) class, which contains the following information:

*   [start_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/start_time/) - the start time of the search;
*   [end_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/end_time/) - the end time of the search;
*   [search_duration](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/search_duration/) - the search duration;
*   [document_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/document_count/) - the number of documents found;
*   [occurrence_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/occurrence_count/) - the total number of occurrences found;
*   [text_query](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/text_query/) - the search query in text form, or None if the search was performed with a query in object form;
*   [object_query](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/object_query/) - the search query in object form, or None if the search was performed with a query in text form;
*   [search_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/search_options/) - the search options.

The [to_string](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchreport/to_string/) method returns the search query in text form for both kinds of queries.

The following example demonstrates how to get search reports from an index.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Searching in index
result1 = index.search("Einstein")
result2 = index.search('"Theory of Relativity"')

# Getting search reports
reports = index.get_search_reports()

# Printing reports to the console
for report in reports:
    print("Query: " + report.to_string())
    print("Time: " + str(report.start_time))
    print("Duration: " + str(report.search_duration))
    print("Documents: " + str(report.document_count))
    print("Occurrences: " + str(report.occurrence_count))
    print()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-reports/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-reports/documents/relativity-primer.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-reports.txt" >}}  
```text
Query: Einstein
Time: 2026-10-29 19:06:16.346091+02:00
Duration: 0:00:00.020339
Documents: 1
Occurrences: 3

Query: "Theory of Relativity"
Time: 2026-10-29 19:06:16.366760+02:00
Duration: 0:00:00.000273
Documents: 2
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/search-reports/search_reports/search-reports.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
