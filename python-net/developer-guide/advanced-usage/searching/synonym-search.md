---
id: synonym-search
url: search/python-net/synonym-search
title: Synonym search
weight: 27
description: "This article describes synonym search, which allows you to find not only the words specified in the search query, but also their synonyms."
keywords: Synonym search
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Synonym search allows you to find not only the words specified in the search query, but also their synonyms, that is, words that mean the same.

To enable synonym search, you must set the [use_synonym_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/use_synonym_search/) property of the [SearchOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/) class to True. By default, synonym search is disabled. Synonym search cannot be combined with case-sensitive search. If synonym search is enabled together with case-sensitive search, or if the synonym dictionary of the index is empty, the search returns an empty result and the error is reported through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event (see [Search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}})).

The default synonym dictionary contains synonyms only for the English language. To manage the synonym dictionary, see the [Synonym dictionary]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/synonym-dictionary.md" >}}) page in the [Managing dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The following example demonstrates the synonym search.

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
options.use_synonym_search = True  # Enabling synonym search

# Search for the word 'answer'
# In addition to the word 'answer', the words 'reply' and 'response' will also be found
result = index.search("answer", options)

# Printing the words found in each document
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/synonym-search/documents/quantum-theory.txt), [documents/correspondence.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/synonym-search/documents/correspondence.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "synonym-search.txt" >}}  
```text
Documents found: 1
    correspondence.txt ['reply', 'response']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/synonym-search/synonym_search/synonym-search.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
