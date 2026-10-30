---
id: stop-word-dictionary
url: search/python-net/stop-word-dictionary
title: Stop word dictionary
weight: 7
description: "This article describes the Python API for managing the stop word dictionary of an index."
keywords: stop word dictionary, stop words
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The [StopWordDictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/stopworddictionary/) class is designed to store stop words in an index. Stop words are excluded from indexing only if the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) index setting is enabled. Information on using stop words during indexing is provided on the [Indexing with stop words]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-with-stop-words.md" >}}) page.

{{< alert style="info" >}}Stop words define which terms of documents are indexed. Therefore, the stop word dictionary and the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) setting of an index can be changed only while the index contains no documents and is not performing an operation; otherwise, a change raises `RuntimeError`. A call that does not change the content of the dictionary is allowed. Configure stop words after creating a new index and before adding documents. See also the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

The dictionary keeps words in lower case without leading and trailing white space. Therefore, the letter case of words does not matter when words are added, removed or checked.

To get the number of stop words in the dictionary, use the [count](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/stopworddictionary/count/) property.

To add stop words to the dictionary, use the [add_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/stopworddictionary/add_range/) method.

To remove words from the dictionary, use the [remove_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/stopworddictionary/remove_range/) method.

To check for a word in the dictionary, use the [contains](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/stopworddictionary/contains/) method. The method ignores the letter case of the word.

To remove all words from the dictionary, use the [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/stopworddictionary/clear/) method.

To export words to a file, use the [export_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/export_dictionary/) method.

To import words from a file, use the [import_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/import_dictionary/) method. Errors of exporting and importing are reported by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the stop word dictionary.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings

# Creating an index settings instance
settings = IndexSettings()
settings.use_stop_words = True  # Enabling the use of stop words during indexing

# Creating a new index in the specified folder; the dictionary can be changed only while the index contains no documents
index = Index("./index", settings, True)

stop_words = index.dictionaries.stop_word_dictionary

if stop_words.count > 0:
    # Removing all words from the dictionary
    stop_words.clear()

# Adding stop words to the dictionary
words = ["a", "an", "the", "but", "by"]
stop_words.add_range(words)

# The letter case of words is ignored
if stop_words.contains("But") and stop_words.contains("BY"):
    # Removing words from the dictionary
    stop_words.remove_range(["but", "by"])

# Exporting words to a file
stop_words.export_dictionary("./StopWords.txt")

# Importing words from a file
stop_words.import_dictionary("./StopWords.txt")
print("Stop words:", stop_words.count)

# Indexing documents; after that, the stop words of the index cannot be changed
index.add("./documents")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "stop-word-dictionary.txt" >}}  
```text
Stop words: 3
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary/stop_word_dictionary/stop-word-dictionary.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
