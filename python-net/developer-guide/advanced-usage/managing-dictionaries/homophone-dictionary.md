---
id: homophone-dictionary
url: search/python-net/homophone-dictionary
title: Homophone dictionary
weight: 5
description: "This article describes the Python API for managing the homophone dictionary of an index."
keywords: homophone dictionary, homophones
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The [HomophoneDictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/homophonedictionary/) class is designed to store homophones in an index. For information on searching using homophones, see the [Homophone search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/homophone-search.md" >}}) page.

The homophone dictionary can be changed at any time, also in an index that contains documents. The dictionary keeps words in lower case without leading and trailing white space, so the methods that get homophones ignore the letter case of the specified word.

To get the number of words in the dictionary, use the [count](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/homophonedictionary/count/) property.

To add groups of homophones to the dictionary, use the [add_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/homophonedictionary/add_range/) method.

The [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/homophonedictionary/clear/) method is used to remove all homophones from the dictionary.

The [get_homophones](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/homophonedictionary/get_homophones/) method is used to get a list of homophones for a given word. The returned list does not contain the word itself.

The [get_homophone_groups](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/homophonedictionary/get_homophone_groups/) method is used to get all homophone groups to which a given word belongs.

To get all homophone groups from the dictionary, use the [get_all_homophone_groups](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/homophonedictionary/get_all_homophone_groups/) method.

To export homophones to a file, use the [export_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/export_dictionary/) method.

To import homophones from a file, use the [import_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/import_dictionary/) method. Errors of exporting and importing are reported by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the homophone dictionary.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating or opening an index in the specified folder
index = Index("./index")

homophones = index.dictionaries.homophone_dictionary

# Getting homophones for the word 'braid'; the letter case of the word is ignored
words = homophones.get_homophones("Braid")
print("Homophones for 'braid':")
for word in words:
    print(word)

# Getting groups of homophones to which the word 'braid' belongs
groups = homophones.get_homophone_groups("braid")
print("Homophone groups for 'braid':")
for group in groups:
    print(" ".join(group))

if homophones.count > 0:
    # Removing all homophones from the dictionary
    homophones.clear()

# Adding homophones to the dictionary
homophone_groups = [
    ["awe", "oar", "or", "ore"],
    ["aye", "eye", "i"],
]
homophones.add_range(homophone_groups)
print("Words in the dictionary:", homophones.count)

# Exporting homophones to a file
homophones.export_dictionary("./Homophones.dat")

# Importing homophones from a file
homophones.import_dictionary("./Homophones.dat")
```
{{< /tab >}}
{{< tab "homophone-dictionary.txt" >}}  
```text
Homophones for 'braid':
brayed
Homophone groups for 'braid':
braid brayed
Words in the dictionary: 7
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/homophone-dictionary/homophone_dictionary/homophone-dictionary.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
