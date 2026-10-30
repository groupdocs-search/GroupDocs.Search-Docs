---
id: homophone-search
url: search/python-net/homophone-search
title: Homophone search
weight: 9
description: "This article describes homophone search, which allows you to find not only the words specified in the search query, but also their homophones."
keywords: Homophone search
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Homophone search, or in other words phonic search, allows you to find not only the words specified in the search query, but also their homophones, that is, words that are pronounced the same but differ in meaning.

To enable homophone search, you must set the [use_homophone_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/use_homophone_search/) property of the [SearchOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/) class to True. By default, homophone search is disabled. Homophone search cannot be combined with case-sensitive search. If homophone search is enabled together with case-sensitive search, or if the homophone dictionary of the index is empty, the search returns an empty result and the error is reported through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event (see [Search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}})).

The default homophone dictionary contains homophones only for the English language. To manage the homophone dictionary, see the [Homophone dictionary]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/homophone-dictionary.md" >}}) page in the [Managing dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The following example demonstrates the homophone search.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Creating a search options object
options = SearchOptions()
options.use_homophone_search = True  # Enabling homophone search

# Search for the word 'coal'
# In addition to the word 'coal', the words 'cole' and 'kohl' will also be found
result = index.search("coal", options)

# Printing the words found in each document
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/homophone-search/documents/quantum-theory.txt), [documents/coal-mining.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/homophone-search/documents/coal-mining.txt), [documents/cosmetics-history.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/homophone-search/documents/cosmetics-history.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "homophone-search.txt" >}}  
```text
Documents found: 2
    coal-mining.txt ['coal', 'cole']
    cosmetics-history.txt ['kohl']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/homophone-search/homophone_search/homophone-search.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
