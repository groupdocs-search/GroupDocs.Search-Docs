---
id: alias-dictionary
url: search/python-net/alias-dictionary
title: Alias dictionary
weight: 1
description: "This article describes the Python API for managing the alias dictionary of an index."
keywords: alias dictionary, aliases
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
An instance of the [AliasDictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/aliasdictionary/) class contains all the aliases defined in an index. Information on using aliases to search is provided on the [Using aliases]({{< ref "search/python-net/developer-guide/advanced-usage/searching/using-aliases.md" >}}) page.

To get the number of existing aliases, use the [count](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/aliasdictionary/count/) property.

Use the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/aliasdictionary/add/) method to add a new alias-replacement pair. An alias can contain only Latin letters and digits; the leading @ character is optional. The dictionary keeps aliases in lower case, and its methods ignore the letter case of aliases; in a search query, write an alias in lower case.

Use the [add_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/aliasdictionary/add_range/) method to add a collection of new alias-replacement pairs. It takes a list of [AliasReplacementPair](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/aliasreplacementpair/) objects.

To remove an alias from the dictionary, use the [remove](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/aliasdictionary/remove/) method.

The [contains](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/aliasdictionary/contains/) method is used to check for the presence of a particular alias in the dictionary.

To get a replacement for a particular alias, use the [get_text](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/aliasdictionary/get_text/) method.

The [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/aliasdictionary/clear/) method is used to remove all existing aliases from the dictionary.

The alias dictionary can be changed at any time, also in an index that contains documents.

To export the list of aliases with replacements to a file, use the [export_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/export_dictionary/) method.

To import the list of aliases from a file, use the [import_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/import_dictionary/) method. Errors of exporting and importing are reported by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the alias dictionary.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.dictionaries import AliasReplacementPair

# Creating or opening an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

aliases = index.dictionaries.alias_dictionary

if aliases.count > 0:
    # Deleting all existing aliases
    aliases.clear()

# Adding aliases to the alias dictionary
aliases.add("t", "(theory OR relativity)")
aliases.add("e", "(Einstein OR Albert)")
pairs = [
    AliasReplacementPair("d", "daterange(2017-01-01 ~~ 2019-12-31)"),
    AliasReplacementPair("n", "(100 ~~ 900)"),
]
aliases.add_range(pairs)

if aliases.contains("e"):
    # Getting an alias replacement
    replacement = aliases.get_text("e")
    print("e - " + replacement)

# Exporting aliases to a file
aliases.export_dictionary("./Aliases.dat")

# Importing aliases from a file
aliases.import_dictionary("./Aliases.dat")

# Searching in the index
result = index.search("@t OR @e")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/alias-dictionary/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/alias-dictionary/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/alias-dictionary/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "alias-dictionary.txt" >}}  
```text
e - (Einstein OR Albert)
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/alias-dictionary/alias_dictionary/alias-dictionary.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
