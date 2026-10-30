---
id: word-forms-provider
url: search/python-net/word-forms-provider
title: Word forms provider
weight: 10
description: "This article describes the IWordFormsProvider interface and how to set a custom provider of word forms for an index in GroupDocs.Search for Python via .NET."
keywords: word forms provider, IWordFormsProvider, word forms
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The [IWordFormsProvider](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/iwordformsprovider/) interface is designed to implement a word forms provider for searching by word forms. For information on searching by word forms, see the [Search for different word forms]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}) page.

The [IWordFormsProvider](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/iwordformsprovider/) interface contains only one [get_word_forms](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/iwordformsprovider/get_word_forms/) method, which returns various forms for the word passed as an argument. In Python, a provider is a plain class with the `get_word_forms(word)` method that returns a list of strings; a function that takes the word also works. An example implementation of a simple provider of word forms is presented below.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
class SimpleWordFormsProvider:
    def get_word_forms(self, word):
        result = []
        lower_word = word.lower()

        # We assume that the input word is in the plural, then we add the singular
        if len(word) > 3 and lower_word.endswith("ies"):
            result.append(word[:-3] + "y")
        if len(word) > 2 and lower_word.endswith("es"):
            result.append(word[:-2])
        if len(word) > 1 and lower_word.endswith("s"):
            result.append(word[:-1])

        # Then we assume that the input word is in the singular, we add the plural
        if len(word) > 1 and lower_word.endswith("y"):
            result.append(word[:-1] + "ies")
        elif lower_word.endswith(("s", "x", "ch", "sh")):
            result.append(word + "es")
        else:
            result.append(word + "s")
        # All rules are implemented in the EnglishWordFormsProvider class

        return result
```
{{< /tab >}}
{{< /tabs >}}

By default, the **[EnglishWordFormsProvider](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/englishwordformsprovider/)** class is used, which for English generates various forms of nouns, adjectives, pronouns, verbs, etc. A custom provider is set with the [word_forms_provider](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionaryrepository/word_forms_provider/) property of the [Index.dictionaries](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/dictionaries/) object. The provider can be replaced at any time; it is not saved in the index, so it must be set each time the index is opened. An example of setting a custom provider of word forms is presented below.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import SearchOptions


class SimpleWordFormsProvider:
    def get_word_forms(self, word):
        result = []
        lower_word = word.lower()

        # We assume that the input word is in the plural, then we add the singular
        if len(word) > 3 and lower_word.endswith("ies"):
            result.append(word[:-3] + "y")
        if len(word) > 2 and lower_word.endswith("es"):
            result.append(word[:-2])
        if len(word) > 1 and lower_word.endswith("s"):
            result.append(word[:-1])

        # Then we assume that the input word is in the singular, we add the plural
        if len(word) > 1 and lower_word.endswith("y"):
            result.append(word[:-1] + "ies")
        elif lower_word.endswith(("s", "x", "ch", "sh")):
            result.append(word + "es")
        else:
            result.append(word + "s")

        return result


# Creating or opening an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Setting the custom word forms provider instance
index.dictionaries.word_forms_provider = SimpleWordFormsProvider()

# Creating a search options instance
options = SearchOptions()
options.use_word_forms_search = True  # Enabling search for word forms

# Searching in the index
result = index.search("relative", options)

# The following words can be found:
# relative
# relatives
for document in result:
    print(document.document_info.file_path)
    print("\tFound words:", ", ".join(document.terms))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/word-forms-provider/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/word-forms-provider/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/word-forms-provider/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "word-forms-provider-2.txt" >}}  
```text
documents\einstein-notes.txt
	Found words: relative, relatives
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/word-forms-provider/word_forms_provider_2/word-forms-provider-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
