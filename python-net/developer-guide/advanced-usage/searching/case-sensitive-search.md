---
id: case-sensitive-search
url: search/python-net/case-sensitive-search
title: Case sensitive search
weight: 2
description: "This article describes case-sensitive search, which allows you to find words considering uppercase and lowercase letters as distinct."
keywords: Case sensitive search 
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Case-sensitive search allows you to find words considering uppercase and lowercase letters as distinct. For example, in the context of case-sensitive search, the words 'Theory', 'theory', and 'THEORY' are all different.

Note that case-sensitive search cannot be combined with fuzzy search, synonym search, homophone search, word forms search, spelling correction, and keyboard layout correction (see [Search flow]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-flow.md" >}})). If case-sensitive search is enabled together with any of these options, the search does not throw an exception: it returns an empty result and raises the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index with a message describing the conflict. For details, see the [Search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}}) page.

The following example demonstrates how to perform case-sensitive search with a query in text form.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchOptions

index = Index("./index")  # Creating index in the specified folder
index.add("./documents")  # Indexing documents from the specified folder

options = SearchOptions()
options.use_case_sensitive_search = True  # Enabling case sensitive search

query = "Windows"
result = index.search(query, options)  # Searching

# Printing the documents found
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/operating-systems.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/case-sensitive-search/documents/operating-systems.txt), [documents/house-renovation.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/case-sensitive-search/documents/house-renovation.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "case-sensitive-search.txt" >}}  
```text
Documents found: 1
    operating-systems.txt ['Windows']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/case-sensitive-search/case_sensitive_search/case-sensitive-search.txt)
{{< /tab >}}
{{< /tabs >}}

The next example demonstrates how to perform case-sensitive search with a query in object form.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery
from groupdocs.search.options import SearchOptions

index = Index("./index")  # Creating index in the specified folder
index.add("./documents")  # Indexing documents from the specified folder

options = SearchOptions()
options.use_case_sensitive_search = True  # Enabling case sensitive search

query = SearchQuery.create_word_query("Windows")  # Creating search query in object form

result = index.search(query, options)  # Searching

# Printing the documents found
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/operating-systems.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/case-sensitive-search/documents/operating-systems.txt), [documents/house-renovation.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/case-sensitive-search/documents/house-renovation.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "case-sensitive-search-2.txt" >}}  
```text
Documents found: 1
    operating-systems.txt ['Windows']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/case-sensitive-search/case_sensitive_search_2/case-sensitive-search-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
