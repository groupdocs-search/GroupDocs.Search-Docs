---
id: numeric-range-search
url: search/python-net/numeric-range-search
title: Numeric range search
weight: 12
description: "This article describes numeric range search, which allows you to search documents for integer numbers in the range from 0 to 9223372036854775807 (Int64.MaxValue)."
keywords: numeric range search
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Numeric range search or numerical search allows you to search documents for integer numbers in the range from 0 to 9223372036854775807 (Int64.MaxValue). Please note that a number in the text must not be separated by spaces; otherwise, it is treated as several numbers. A search query of this type is specified as follows:

number ~~ number

The example below demonstrates the numeric range search in text and object forms.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search with text query
result1 = index.search("500 ~~ 600")

# Search with object query
query2 = SearchQuery.create_numeric_range_query(500, 600)
result2 = index.search(query2)

# Printing the numbers found in each document
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/library-stats.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/numeric-range-search/documents/library-stats.txt), [documents/launch-plan.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/numeric-range-search/documents/launch-plan.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "numeric-range-search.txt" >}}  
```text
Documents found by the text query: 1
Documents found by the object query: 1
    library-stats.txt ['550']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/numeric-range-search/numeric_range_search/numeric-range-search.txt)
{{< /tab >}}
{{< /tabs >}}

Since version 26.10, numeric range search uses as many threads as there are processor cores by default. Therefore, the order of the found numbers in the search result may differ from run to run. To get the same order every time, set the [search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/search_threads/) index setting to NumberOfThreads.ONE.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
