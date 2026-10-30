---
id: indexing-with-stop-words
url: search/python-net/indexing-with-stop-words
title: Indexing with stop words
weight: 18
description: "This article explains how to enable or disable the use of stop words during indexing."
keywords: stop words, indexing with stop words, use_stop_words, stop word dictionary, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Stop words are frequently used words that do not carry a semantic meaning and can be removed from an index to reduce its size.

You can enable or disable the use of stop words by setting a value of the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) property of the [IndexSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/) class. The default value is `True`, meaning that stop words are filtered during indexing and not added to an index.

A list of stop words to use during indexing can be specified in the stop word dictionary. By default, the stop word dictionary is filled with the most widely used pronouns, prepositions and conjunctions of English and Russian. The list of stop words can be replaced or supplemented, and it is saved in the index. Stop words are kept in lower case, and the lookups in the stop word dictionary ignore letter case. For information on managing the stop word dictionary, see the [Stop word dictionary]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary.md" >}}) page in the [Managing dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

Since version 26.10, the stop word dictionary and the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) setting can be changed only while the index contains no documents and is not performing an operation; otherwise, `RuntimeError` is raised. Therefore, configure them after creating a new index and before adding the first document, as the example below does. To use other stop words with documents that are already indexed, create a new index and index the documents again. See the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page for details.

If you need to keep all text information extracted from documents, and you are not afraid of a significant increase in the size of the index, then an example of indexing without stop words can be found below.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings

# Creating index settings with the use of stop words disabled
settings = IndexSettings()
settings.use_stop_words = False

# Creating a new index in the specified folder
# The use_stop_words setting can be changed only while the index contains no documents
index = Index("./index", settings, True)

# Indexing documents from the specified folder
index.add("./documents")

# Searching in the index
# Now in the index it is possible to search for the stop word 'on'
result = index.search("on")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-with-stop-words/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-with-stop-words/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-with-stop-words.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-with-stop-words/indexing_with_stop_words/indexing-with-stop-words.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
