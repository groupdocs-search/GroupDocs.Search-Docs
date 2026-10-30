---
id: synonym-dictionary
url: search/python-net/synonym-dictionary
title: Synonym dictionary
weight: 8
description: "This article describes the Python API for managing the synonym dictionary of an index."
keywords: synonym dictionary, synonyms
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The [SynonymDictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/synonymdictionary/) class is designed to store synonyms in an index. For information on searching using synonyms, see the [Synonym search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/synonym-search.md" >}}) page.

The synonym dictionary can be changed at any time, also in an index that contains documents. The dictionary keeps words in lower case without leading and trailing white space, so the methods that get synonyms ignore the letter case of the specified word.

To get the number of words in the dictionary, use the [count](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/synonymdictionary/count/) property.

To add groups of synonyms to the dictionary, use the [add_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/synonymdictionary/add_range/) method.

The [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/synonymdictionary/clear/) method is used to remove all synonyms from the dictionary.

The [get_synonyms](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/synonymdictionary/get_synonyms/) method is used to get a list of synonyms for a given word. The returned list does not contain the word itself.

The [get_synonym_groups](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/synonymdictionary/get_synonym_groups/) method is used to get all synonym groups to which a given word belongs.

To get all synonym groups from the dictionary, use the [get_all_synonym_groups](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/synonymdictionary/get_all_synonym_groups/) method.

To export synonyms to a file, use the [export_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/export_dictionary/) method.

To import synonyms from a file, use the [import_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/import_dictionary/) method. Errors of exporting and importing are reported by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the synonym dictionary.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating or opening an index in the specified folder
index = Index("./index")

synonyms = index.dictionaries.synonym_dictionary

# Getting synonyms for the word 'make'; the letter case of the word is ignored
words = synonyms.get_synonyms("Make")
print("Synonyms for 'make':")
for word in words:
    print(word)

# Getting groups of synonyms to which the word 'make' belongs
groups = synonyms.get_synonym_groups("make")
print("Synonym groups for 'make':")
for group in groups:
    print(" ".join(group))

if synonyms.count > 0:
    # Removing all synonyms from the dictionary
    synonyms.clear()

# Adding synonyms to the dictionary
synonym_groups = [
    ["achieve", "accomplish", "attain", "reach"],
    ["accept", "take", "have"],
]
synonyms.add_range(synonym_groups)
print("Words in the dictionary:", synonyms.count)

# Exporting synonyms to a file
synonyms.export_dictionary("./Synonyms.dat")

# Importing synonyms from a file
synonyms.import_dictionary("./Synonyms.dat")
```
{{< /tab >}}
{{< tab "synonym-dictionary.txt" >}}  
```text
Synonyms for 'make':
brand
construct
build
cook
fix
ready
prepare
induce
stimulate
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/synonym-dictionary/synonym_dictionary/synonym-dictionary.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
