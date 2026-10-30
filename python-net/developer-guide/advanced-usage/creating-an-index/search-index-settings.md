---
id: search-index-settings
url: search/python-net/search-index-settings
title: Search index settings
weight: 3
description: "This page contains a description of all index settings that can be specified in an instance of the IndexSettings class in GroupDocs.Search for Python via .NET."
keywords: index settings, description of all index settings
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains a description of all index settings that can be specified in an instance of the [IndexSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/) class.

An index settings object is passed to an [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) constructor when a new index is created. The settings are saved in the index. When an existing index is opened, it uses the settings saved in it; only the [custom_extractors](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/custom_extractors/) and [logger](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/logger/) settings, which are not saved, are taken from the passed settings object. The settings of an opened index are available through the [Index.index_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/index_settings/) property.

Some settings define how the text of documents is divided into terms, so they cannot be changed freely after the index is created:

*   The [index_type](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/index_type/), [text_storage_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/text_storage_settings/), [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) settings are fixed when the index is created; a value assigned after that is ignored.
*   The [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) and [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/) settings can be changed only while the index contains no documents and is not performing an operation; otherwise, a change raises `RuntimeError`.
*   The other settings can be changed at any time.

For details, see the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

## custom_extractors property

The [custom_extractors](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/custom_extractors/) property returns a collection of custom text extractors that allows adding new extractors for supported or not supported formats. Custom extractors are not saved in an index, so they must be added each time the index is created or opened. A complete example of implementing a custom text extractor and using it to extract text is presented on the page [Custom text extractors]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}).

## document_filter property

The [document_filter](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/document_filter/) property allows you to set a filter used to determine whether files being added to the index should be indexed. If a document added separately or located in an added folder does not match the filter, it is not indexed. The default value is `None`, which means that all added files will be indexed if their format is supported. The value can be changed at any time. Detailed information on creating and setting the document indexing filter can be found on the page [Document filtering during indexing]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}}).

## index_type property

The [index_type](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/index_type/) property is used to specify the type of an index. The [IndexType](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indextype/) enumeration contains 3 values:

*   **NORMAL_INDEX** - the type of an index containing the content and metadata of the added documents. It supports all search features.
*   **METADATA_INDEX** - the type of an index containing only metadata of added documents. It supports all search features. An example of creating an index of this type is presented on the [Indexing metadata of documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-metadata-of-documents.md" >}}) page.
*   **COMPACT_INDEX** - the type of an index containing the content and metadata of the added documents. It takes much less disk space, but does not support the [phrase search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/phrase-search.md" >}}) and the [date range search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/date-range-search.md" >}}) features.

The default value of this property is **NORMAL_INDEX**. The index type is fixed when the index is created.

## in_memory_index property

The read-only property [in_memory_index](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/in_memory_index/) returns a value indicating whether an index is located in RAM or on disk. The value of this property is set indirectly when creating or loading an index. An index is created on disk when the path to the index folder is specified; otherwise, an index is created in memory. See the [Search index location]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-location.md" >}}) page for details.

## logger property

The [logger](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/logger/) property allows you to specify in the index settings the logger used for logging index events and errors during its operation. The default value is `None` meaning that logging is not used. The logger is not saved in an index, so it must be assigned each time the index is created or opened. Detailed information on creating and assigning an index logger and the implementation of a custom logger is presented on the page [Logging]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/logging.md" >}}).

## max_indexing_report_count property

The [max_indexing_report_count](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/max_indexing_report_count/) property allows you to specify the maximum number of indexing reports stored in RAM for an index since it was created or loaded. The default value is 5. Detailed information on indexing reports is provided on the page [Indexing reports]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-reports.md" >}}).

## max_search_report_count property

The [max_search_report_count](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/max_search_report_count/) property allows you to specify the maximum number of search reports stored in RAM for an index since it was created or loaded. The default value is 10. Detailed information on search reports is provided on the page [Search reports]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-reports.md" >}}).

## replace_unpaired_surrogates property

The [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) property specifies how unpaired UTF-16 surrogate characters in the text of documents are processed during indexing. If the value is `True`, each unpaired surrogate character is replaced with the Unicode replacement character (U+FFFD), so the characters around it are not joined into a single term. If the value is `False`, unpaired surrogate characters are removed, as previous versions did.

The default value is `True`. The setting is fixed when the index is created. An index updated from a version earlier than 26.10 has this setting disabled.

## search_threads property

The [search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/search_threads/) property allows you to set the number of threads used to search in an index. By default, this value is [NumberOfThreads](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/numberofthreads/).DEFAULT, which means that the search is performed using as many threads as there are processor cores. This number of threads ensures the optimal speed of each individual search in an index. If you plan a large number of parallel search queries and you need to ensure maximum total search performance, you should set the [NumberOfThreads](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/numberofthreads/).ONE value for this property.

The value can be changed at any time, also for an existing index through the [Index.index_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/index_settings/) property. A search uses the value that is set when the search starts, and the new value is saved in the index. The setter raises `ValueError` if the value is not a member of the [NumberOfThreads](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/numberofthreads/) enumeration.

In a multithreaded search, the order of the words found by a regular expression, numeric range, fuzzy or spelling correction search may differ from run to run. If you need a deterministic order of the results, use the [NumberOfThreads](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/numberofthreads/).ONE value.

The following example demonstrates how to change the number of search threads of an existing index.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import NumberOfThreads

# Creating an index, indexing documents and closing the index
index = Index("./index")
index.add("./documents")
index.dispose()

# Opening the existing index
index = Index("./index")

# Changing the number of threads used for the search; the new value is saved in the index
index.index_settings.search_threads = NumberOfThreads.ONE

# Searching using the new number of threads
result = index.search("Einstein")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-settings/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-settings/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-settings/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-threads-property.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-settings/search_threads_property/search-threads-property.txt)
{{< /tab >}}
{{< /tabs >}}

## skip_blended_only_terms property

The [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) property specifies whether terms that consist only of [blended characters]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/character-types.md#blended-characters" >}}) are skipped during indexing. For example, if the hyphen is a blended character, a standalone hyphen in the text is not indexed as a term when the value is `True`. This reduces the number of meaningless terms in the index.

The default value is `True`. The setting is fixed when the index is created. An index updated from a version earlier than 26.10 has this setting disabled.

The following example demonstrates how to create an index that keeps the terms consisting only of blended characters.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings

# Creating index settings
settings = IndexSettings()
settings.skip_blended_only_terms = False  # Keeping the terms consisting only of blended characters

# Creating an index with the specified settings
index = Index("./index", settings, True)

# Indexing documents from the specified folder
index.add("./documents")

print("Skip blended-only terms:", index.index_settings.skip_blended_only_terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-settings/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-settings/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-settings/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "skip-blended-only-terms-property.txt" >}}  
```text
Skip blended-only terms: False
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-settings/skip_blended_only_terms_property/skip-blended-only-terms-property.txt)
{{< /tab >}}
{{< /tabs >}}

## text_storage_settings property

The [text_storage_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/text_storage_settings/) property allows you to specify the settings for saving the text of indexed documents in an index. The default value is `None`, which means that document texts are not stored. The setting is fixed when the index is created. Detailed information on saving the text of indexed documents in an index is presented on the page [Storing text of indexed documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}).

## use_character_replacements property

The [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/) property allows you to set the value indicating whether to perform character replacements during the indexing process or not. The default value is `False`. The value can be changed only while the index contains no documents and is not performing an operation; otherwise, `RuntimeError` is raised. Details on replacing characters during indexing are provided on the page [Character replacement during indexing]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/character-replacement-during-indexing.md" >}}).

## use_stop_words property

The [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) property allows you to specify a value indicating whether to use stop words during indexing or not. Stop words are frequently used words that do not carry a semantic load, which are removed from an index to reduce its size. The default value for this property is `True`. The value can be changed only while the index contains no documents and is not performing an operation; otherwise, `RuntimeError` is raised. Detailed information on stop words and their use is presented on the page [Indexing with stop words]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-with-stop-words.md" >}}).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
