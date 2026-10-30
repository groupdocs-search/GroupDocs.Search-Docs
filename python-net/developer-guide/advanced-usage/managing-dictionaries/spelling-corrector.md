---
id: spelling-corrector
url: search/python-net/spelling-corrector
title: Spelling corrector
weight: 6
description: "This article describes the Python API for managing the spelling corrector dictionary of an index."
keywords: spelling corrector, spell checking dictionary
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The [SpellingCorrector](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/spellingcorrector/) class is designed to correct spelling errors in search queries, as well as to store words with correct spelling. You can learn about spelling correction in search queries on the [Spell checking]({{< ref "search/python-net/developer-guide/advanced-usage/searching/spell-checking.md" >}}) page.

The spelling corrector dictionary can be changed at any time, also in an index that contains documents.

To get the number of words in the spelling corrector dictionary, use the [count](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/spellingcorrector/count/) property.

To get a list of words contained in the spelling corrector dictionary, the [get_words](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/spellingcorrector/get_words/) method is used.

To add words to the spelling corrector dictionary, use the [add_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/spellingcorrector/add_range/) method.

The [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/spellingcorrector/clear/) method is used to remove all words from the spelling corrector dictionary.

To export words to a file, use the [export_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/export_dictionary/) method.

To import words from a file, use the [import_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/import_dictionary/) method. Errors of exporting and importing are reported by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the spelling corrector.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating or opening an index in the specified folder
index = Index("./index")

corrector = index.dictionaries.spelling_corrector

if corrector.count > 0:
    # Removing all words from the dictionary
    corrector.clear()

# Adding words to the dictionary
words = ["achieve", "accomplish", "attain", "reach"]
corrector.add_range(words)

# Getting all words of the dictionary
all_words = corrector.get_words()
print("Words in the dictionary:", len(all_words))

# Exporting words to a file
corrector.export_dictionary("./Words.txt")

# Importing words from a file
corrector.import_dictionary("./Words.txt")
```
{{< /tab >}}
{{< tab "spelling-corrector.txt" >}}  
```text
Words in the dictionary: 4
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/spelling-corrector/spelling_corrector/spelling-corrector.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
