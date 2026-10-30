---
id: fuzzy-search
url: search/python-net/fuzzy-search
title: Fuzzy search
weight: 6
description: "This article defines fuzzy search and describes its capabilities in GroupDocs.Search."
keywords: fuzzy search
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page provides a fuzzy search definition, a description of its capabilities in GroupDocs.Search, and fuzzy search Python examples with use of GroupDocs.Search.

## Fuzzy search definition

Fuzzy search is a text search technique based on fuzzy logic that matches even where search queries or the text of documents to search for are written with errors.

## Fuzzy search features

Fuzzy search allows you to search for words that do not exactly match the search query. This type of search can be used, for example, to search for words containing spelling errors in documents. During fuzzy search, the number of possible differences with the search word is determined by the fuzzy search algorithm used. Differences or errors mean insertions, deletions, substitutions and, optionally, transpositions of adjacent characters.

GroupDocs.Search does not use special fuzzy queries. Instead, you can enable fuzzy search in search options for any search query, whether it is a single word, phrase or logical search query. And it becomes a fuzzy query.

The fuzzy search algorithm is defined by the [fuzzy_algorithm](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/fuzzysearchoptions/fuzzy_algorithm/) property of the fuzzy search options object returned by the [SearchOptions.fuzzy_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/fuzzy_search/) property. By default, the algorithm provided by the [SimilarityLevel](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/similaritylevel/) class is used with the similarity level value of 0.5. This algorithm uses a linear dependence of the number of possible differences from the search word. To calculate the maximum number of possible differences, the following expression is used:

max_mistake_count = int((1 - similarity_level) \* term_length)

where similarity_level is a parameter of the fuzzy search algorithm; term_length is the length of the searched word. A similarity level less than 0 is treated as 0, and a similarity level greater than 1 is treated as 1.

An example of setting a fuzzy search algorithm of this type is presented below.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchOptions, SimilarityLevel

query = "Einstein"

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

options = SearchOptions()
options.fuzzy_search.enabled = True  # Enabling the fuzzy search
options.fuzzy_search.fuzzy_algorithm = SimilarityLevel(0.75)  # Creating the fuzzy search algorithm
# This function specifies 0 as the maximum number of mistakes for words from 1 to 3 characters.
# It specifies 1 as the maximum number of mistakes for words from 4 to 7 characters.
# It specifies 2 as the maximum number of mistakes for words from 8 to 11 characters. And so on.

# Search in index
result = index.search(query, options)

# Printing the words found in each document
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/fuzzy-search/documents/einstein-biography.txt), [documents/newspaper-archive.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/fuzzy-search/documents/newspaper-archive.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/fuzzy-search/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "fuzzy-search-features.txt" >}}  
```text
Documents found: 3
    einstein-biography.txt ['einstein']
    newspaper-archive.txt ['einstain', 'einstien']
    nobel-prize.txt ['einstein']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/fuzzy-search/fuzzy_search_features/fuzzy-search-features.txt)
{{< /tab >}}
{{< /tabs >}}

The fuzzy search algorithm can also be specified by a table of correspondences between the length of the searched word and the maximum number of possible differences. For this, the algorithm presented by the **[TableDiscreteFunction](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/tablediscretefunction/)** class is used. The table can be passed to the constructor directly (the offset of the table indices relative to the word length and the list of the maximum numbers of differences), or it can be calculated based on the parameters of a step function. In the latter case, the first argument is the level of the first step, and the second argument is a list of [Step](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/step/) objects; each Step object specifies the lower limit of the word length and the level of the next step. The lower limits must be strictly increasing, otherwise ArgumentException is thrown (ValueError in Python). An example of setting a fuzzy search algorithm in the form of a step function is presented below.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchOptions, Step, TableDiscreteFunction

query = "Einstein"

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

options = SearchOptions()
options.fuzzy_search.enabled = True  # Enabling the fuzzy search
options.fuzzy_search.fuzzy_algorithm = TableDiscreteFunction(1, [Step(5, 2), Step(8, 3)])  # Creating the fuzzy search algorithm
# This function specifies 1 as the maximum number of mistakes for words from 1 to 4 characters.
# It specifies 2 as the maximum number of mistakes for words from 5 to 7 characters.
# It specifies 3 as the maximum number of mistakes for words from 8 and more characters.

# Search in index
result = index.search(query, options)

# Printing the words found in each document
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/fuzzy-search/documents/einstein-biography.txt), [documents/newspaper-archive.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/fuzzy-search/documents/newspaper-archive.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/fuzzy-search/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "fuzzy-search-features-2.txt" >}}  
```text
Documents found: 3
    einstein-biography.txt ['einstein']
    newspaper-archive.txt ['einstain', 'einstien', 'eynstajn']
    nobel-prize.txt ['einstein']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/fuzzy-search/fuzzy_search_features_2/fuzzy-search-features-2.txt)
{{< /tab >}}
{{< /tabs >}}

The fuzzy search options object allows you to specify the following options:

*   The [fuzzy_algorithm](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/fuzzysearchoptions/fuzzy_algorithm/) property defines the fuzzy search algorithm. The currently available fuzzy search algorithms are [SimilarityLevel](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/similaritylevel/) and [TableDiscreteFunction](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/tablediscretefunction/). The default value is an instance of [SimilarityLevel](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/similaritylevel/) class with a similarity level value of 0.5.
*   The [only_best_results](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/fuzzysearchoptions/only_best_results/) property determines whether to return only the best results. The default value is False, which means that all results will be returned.
*   The [only_best_results_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/fuzzysearchoptions/only_best_results_range/) property determines a value of exceeding the number of differences from the best result. The default value is 0.
*   The [consider_transpositions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/fuzzysearchoptions/consider_transpositions/) property determines whether to consider the transposition of adjacent characters as one mistake. The default value is True.
*   The [enabled](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/fuzzysearchoptions/enabled/) property enables or disables the fuzzy search. The default value is False.

Fuzzy search cannot be combined with [case-sensitive search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/case-sensitive-search.md" >}}). If both are enabled, the search returns an empty result and the error is reported through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event.

Since version 26.10, fuzzy search uses as many threads as there are processor cores by default. Therefore, the order of the found words in the search result may differ from run to run. To get the same order every time, set the [search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/search_threads/) index setting to NumberOfThreads.ONE. For details, see the [Search index settings]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md#search_threads-property" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
