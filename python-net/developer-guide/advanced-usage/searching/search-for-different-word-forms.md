---
id: search-for-different-word-forms
url: search/python-net/search-for-different-word-forms
title: Search for different word forms
weight: 20
description: "This article describes the search for different word forms, which allows you to search for nouns in the singular or plural, degrees of comparison of adjectives, forms of regular and irregular verbs, etc."
keywords: Search for different word forms
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The search for different word forms allows you to search for nouns in the singular or plural, degrees of comparison of adjectives, forms of regular and irregular verbs, etc.

Search for different word forms is enabled if the [use_word_forms_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/use_word_forms_search/) property of the [SearchOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/) class is set to True. By default, the search for different word forms is disabled. The search for different word forms cannot be combined with case-sensitive search. If both are enabled, the search returns an empty result and the error is reported through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event (see [Search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}})).

To generate various forms of words, a class that implements the [IWordFormsProvider](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/iwordformsprovider/) interface is used. The default class is [EnglishWordFormsProvider](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/englishwordformsprovider/), which supports English-only word forms. To add support for word forms in other languages, see the [Word forms provider]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/word-forms-provider.md" >}}) page.

The following example demonstrates how to perform search for different word forms in an index.

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

# Creating a search options instance
options = SearchOptions()
options.use_word_forms_search = True  # Enabling search for word forms

# Searching in the index
result = index.search("relative", options)

# The following words can be found:
# relative
# relatives
# relatively

# Printing the words found in each document
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-for-different-word-forms/documents/relativity-primer.txt), [documents/family-visit.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-for-different-word-forms/documents/family-visit.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-different-word-forms.txt" >}}  
```text
Documents found: 1
    family-visit.txt ['relative', 'relatives', 'relatively']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/search-for-different-word-forms/search_for_different_word_forms/search-different-word-forms.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
