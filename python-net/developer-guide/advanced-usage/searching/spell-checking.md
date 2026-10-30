---
id: spell-checking
url: search/python-net/spell-checking
title: Spell checking
weight: 26
description: "This article describes how spell checking works during the search."
keywords: Spell checking
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
During the search, a search query can be checked for spelling errors in words. To enable spelling correction in search queries, the [enabled](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/spellingcorrectoroptions/enabled/) property of the spelling corrector options is set to True. By default, the spelling correction is disabled.

The spelling corrector considers the following types of misprints: adding a character, deleting a character, replacing a character, and, optionally, transposing two adjacent characters.

By default, the spelling corrector dictionary contains only English words. To manage the spelling corrector dictionary, see the [Spelling corrector]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/spelling-corrector.md" >}}) page of the [Managing dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The [SpellingCorrectorOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/spellingcorrectoroptions/) class contains the following properties:

*   [max_mistake_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/spellingcorrectoroptions/max_mistake_count/) is the maximum number of mistakes possible in each word of a search query. The default value is 2. A value greater than 10 is treated as 10, and a negative value is treated as 0.
*   [only_best_results](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/spellingcorrectoroptions/only_best_results/) is a value indicating whether only the best results will be returned by the spelling corrector. The default value is False.
*   [only_best_results_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/spellingcorrectoroptions/only_best_results_range/) is the maximum exceeding of the minimum number of mistakes that are found by the spelling corrector. The default value is 0.
*   [consider_transpositions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/spellingcorrectoroptions/consider_transpositions/) is a value indicating whether the spelling corrector must consider transposition of two adjacent characters as a single mistake (True) or two mistakes (False). The default value is True.

Spelling correction cannot be combined with case-sensitive search. If both are enabled, the search returns an empty result and the error is reported through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event (see [Search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}})).

Since version 26.10, spelling correction uses as many threads as there are processor cores by default. Therefore, the order of the found words in the search result may differ from run to run. To get the same order every time, set the [search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/search_threads/) index setting to NumberOfThreads.ONE.

The following example shows how to perform a search using the spelling correction.

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
options.spelling_corrector.enabled = True  # Enabling the spelling correction
options.spelling_corrector.max_mistake_count = 1  # Setting the maximum number of mistakes
options.spelling_corrector.only_best_results = True  # Enabling the option for only the best results of the spelling correction

# Search for the word "Rleativity" containing a spelling error
# The word "Relativity" will be found that differs from the search query in two transposed letters
result = index.search("Rleativity", options)

# Printing the words found in each document
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/spell-checking/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/spell-checking/documents/relativity-primer.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/spell-checking/documents/quantum-theory.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "spell-checking.txt" >}}  
```text
Documents found: 2
    einstein-biography.txt ['relativity']
    relativity-primer.txt ['relativity']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/spell-checking/spell_checking/spell-checking.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
