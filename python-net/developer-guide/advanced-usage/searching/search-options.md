---
id: search-options
url: search/python-net/search-options
title: Search options
weight: 22
description: "This article describes the search options that can be specified in an instance of the SearchOptions class."
keywords: Search options
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains a description of all search options that can be specified in an instance of the [SearchOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/) class. If no search options are passed to the [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search/) method, the default values described below are used.

## cancellation property

The [cancellation](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/cancellation/) property allows you to specify the object to cancel the search operation. The search can be canceled without delay by calling the [cancel](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/cancellation/cancel/) method or by calling the [cancel_after](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/cancellation/cancel_after/) method with the delay in milliseconds as an argument. The default value is None, which means that the search cannot be canceled. To cancel the continuation of a search by chunks, pass the cancellation object to the [search_next](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search_next/) method (see the [Search by chunks]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) page).

## date_formats property

The [date_formats](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/date_formats/) property allows you to specify formats for the date range search feature. By default, the collection contains the formats 'dd.MM.yyyy', 'MM/dd/yyyy', and 'yyyy-MM-dd'. For details, see the [Date range search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/date-range-search.md" >}}) page.

## fuzzy_search property

The [fuzzy_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/fuzzy_search/) property returns a fuzzy search options object that allows you to enable fuzzy search and set its options. By default, the fuzzy search feature is disabled. For details, see the [Fuzzy search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/fuzzy-search.md" >}}) page.

## is_chunk_search property

The [is_chunk_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/is_chunk_search/) property allows you to enable search by chunks. By default, the search by chunks feature is disabled. For details, see the [Search by chunks]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) page.

## keyboard_layout_corrector property

The [keyboard_layout_corrector](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/keyboard_layout_corrector/) property returns a keyboard layout corrector options object, which allows you to enable keyboard layout correction in search queries. By default, the keyboard layout correction feature is disabled. See the [Keyboard layout correction]({{< ref "search/python-net/developer-guide/advanced-usage/searching/keyboard-layout-correction.md" >}}) page for details.

## max_occurrence_count_per_term property

The [max_occurrence_count_per_term](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/max_occurrence_count_per_term/) property allows you to specify the maximum number of occurrences of each word found that will be returned as a search result. The default value is 2000000000.

## max_total_occurrence_count property

The [max_total_occurrence_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/max_total_occurrence_count/) property allows you to specify the maximum number of occurrences in total for all found words, which will be returned as a search result. The default value is 2000000000.

## retrieve_attributes property

The [retrieve_attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/retrieve_attributes/) property allows you to retrieve the attributes associated with the found documents. If the property is set to True, the attributes of each found document are available in the [FoundDocument.attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/attributes/) property; otherwise, this property returns an empty list. The default value is False. For details on document attributes, see the [Document attributes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) page.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import AttributeChangeBatch
from groupdocs.search.options import SearchOptions

query = "Einstein"

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Associating attributes with one of the indexed documents (see the Document attributes page)
batch = AttributeChangeBatch()
batch.add(os.path.abspath("./documents/einstein-biography.txt"), ["main", "biography"])
index.change_attributes(batch)

# Creating a search options instance
options = SearchOptions()
options.retrieve_attributes = True  # Enabling the retrieval of document attributes

# Search in the index
result = index.search(query, options)

# Printing the attributes of the found documents
for i in range(result.document_count):
    document = result.get_found_document(i)
    print(document.document_info.file_path + ": " + ", ".join(document.attributes))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-options/documents/einstein-biography.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-options/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "retrieve-attributes-property.txt" >}}  
```text
documents\einstein-biography.txt: main, biography
documents\nobel-prize.txt:
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/search-options/retrieve_attributes_property/retrieve-attributes-property.txt)
{{< /tab >}}
{{< /tabs >}}

## search_document_filter property

The [search_document_filter](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/search_document_filter/) property allows you to specify the filter of documents returned as a search result. By default, all documents found will be returned as a search result. For details, see the [Document filtering in search result]({{< ref "search/python-net/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}) page.

## spelling_corrector property

The [spelling_corrector](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/spelling_corrector/) property returns the spelling corrector options object, which allows you to enable spelling correction in search queries and specify spelling correction options. By default, spelling correction is disabled. See the [Spell checking]({{< ref "search/python-net/developer-guide/advanced-usage/searching/spell-checking.md" >}}) page for details.

## use_case_sensitive_search property

The [use_case_sensitive_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/use_case_sensitive_search/) property allows you to perform case-sensitive search. Please note that case-sensitive search cannot be combined with fuzzy search, synonym search, homophone search, word forms search, spelling correction, and keyboard layout correction (see [Incompatible options](#incompatible-options) below). By default, case-insensitive search is performed. For details, see the [Case sensitive search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/case-sensitive-search.md" >}}) page.

## use_homophone_search property

The [use_homophone_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/use_homophone_search/) property allows you to enable the homophone search feature. By default, the homophone search feature is disabled. For details, see the [Homophone search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/homophone-search.md" >}}) page.

## use_synonym_search property

The [use_synonym_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/use_synonym_search/) property allows you to enable the synonym search feature. By default, the synonym search feature is disabled. For details, see the [Synonym search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/synonym-search.md" >}}) page.

## use_word_forms_search property

The [use_word_forms_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/use_word_forms_search/) property allows you to enable the word forms search feature. By default, the word forms search feature is disabled. For details, see the [Search for different word forms]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}) page.

## Incompatible options

The search options are checked when a search starts. In the following cases, the [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search/) method does not throw an exception: it returns an empty search result and raises the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index with a message describing the problem:

*   Case-sensitive search is enabled together with fuzzy search, synonym search, homophone search, word forms search, spelling correction, or keyboard layout correction.
*   Synonym search is enabled, but the synonym dictionary of the index is empty.
*   Homophone search is enabled, but the homophone dictionary of the index is empty.

A search query in text form with a syntax error is handled in the same way: the search returns an empty result, and the error message is passed to the error_occurred event. Therefore, it is recommended to subscribe to the error_occurred event to receive such messages. For details on index events, see the [Search index events]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}) page.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import SearchOptions

query = "Einstein"

# Creating an index in the specified folder
index = Index("./index")

# Subscribing to the event of errors
index.events.error_occurred += lambda sender, args: print(args.message)

# Indexing documents from the specified folder
index.add("./documents")

# Creating a search options instance with incompatible options
options = SearchOptions()
options.use_case_sensitive_search = True
options.fuzzy_search.enabled = True

# The search returns an empty result, and the error_occurred event reports the reason
result = index.search(query, options)
print("Document count: " + str(result.document_count))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-options/documents/einstein-biography.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-options/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "incompatible-options.txt" >}}  
```text
Case-sensitive search is not supported together with fuzzy search.
Document count: 0
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/search-options/incompatible_options/incompatible-options.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
