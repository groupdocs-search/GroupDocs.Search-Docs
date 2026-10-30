---
id: alphabet
url: search/python-net/alphabet
title: Alphabet
weight: 2
description: "This article describes the Python API for managing the alphabet of an index, which defines the types of characters used to divide text into words."
keywords: alphabet, character types, separator, letter, blended character, surrogate pair
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
An instance of the [Alphabet](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/) class contains the types of characters used for indexing. The alphabet of an index is available through the [alphabet](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionaryrepository/alphabet/) property of the [Index.dictionaries](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/dictionaries/) object. For detailed information on the types of characters and the default content of the alphabet, see the [Character types]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/character-types.md" >}}) page.

{{< alert style="info" >}}The alphabet defines how the text of documents is divided into terms. Therefore, the alphabet of an index can be changed only while the index contains no documents and is not performing an operation; otherwise, a method that changes the alphabet raises `RuntimeError`. A call that does not change the content of the alphabet is allowed. Configure the alphabet after creating a new index and before adding documents. See also the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

The [get_character_type](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/get_character_type/) method is used to get the type of a specific character. The character can be specified as a string of one character, also a character outside the Basic Multilingual Plane, or as a Unicode code point (an `int` value). The method returns a value of the [CharacterType](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/charactertype/) enumeration; its `name` attribute gives the name, as the example below shows.

The [count](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/count/) property returns the number of characters of a type other than [CharacterType](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/charactertype/).SEPARATOR. Iterating over an [Alphabet](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/) object returns the Unicode code points of these characters.

To set the type of characters in the alphabet, use the [set_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/set_range/) method. The characters can be passed as a list of one-character strings or as a list of Unicode code points. Characters outside the Basic Multilingual Plane can be passed either way.

To set the [CharacterType](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/charactertype/).SEPARATOR type for all characters in the alphabet, use the [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/clear/) method. Note that after clearing, the alphabet contains no letters, so no words can be indexed. Call this method only when you are going to define all the required letters afterwards, for example, with the [set_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/set_range/) method or by importing an alphabet from a file.

To export types of all characters to a file, use the [export_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/export_dictionary/) method.

To import types of characters from a file, use the [import_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/import_dictionary/) method. The imported types are set for the characters listed in the file; the types of other characters are not changed. To make the alphabet exactly the same as the exported one, call the [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/clear/) method before importing. Errors of exporting and importing are reported by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the alphabet.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.dictionaries import CharacterType

# Creating a new index in the specified folder; the alphabet can be changed only while the index contains no documents
index = Index("./index", True)

alphabet = index.dictionaries.alphabet

# Getting the type of the hyphen character
hyphen_type = alphabet.get_character_type("-")
print("The type of hyphen:", hyphen_type.name)

if hyphen_type != CharacterType.BLENDED:
    # Setting the Blended type for the hyphen character
    alphabet.set_range(["-"], CharacterType.BLENDED)

# Setting the Letter type for characters outside the Basic Multilingual Plane specified as Unicode code points
alphabet.set_range([0x1D400, 0x1D401], CharacterType.LETTER)  # Mathematical bold capital A and B

# Setting the Letter type for a character outside the Basic Multilingual Plane specified as a string
alphabet.set_range(["\U0001D41A"], CharacterType.LETTER)  # Mathematical bold small A

# Getting the types of characters specified as a Unicode code point and as a string
print("The type of U+1D400:", alphabet.get_character_type(0x1D400).name)
print("The type of U+1D41A:", alphabet.get_character_type("\U0001D41A").name)

# Getting the number of characters that are not separators
print("Characters that are not separators:", alphabet.count)

# Exporting the alphabet to a file
alphabet.export_dictionary("./Alphabet.dat")

# Indexing documents; after that, the alphabet of the index cannot be changed
index.add("./documents")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/alphabet/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/alphabet/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/alphabet/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "alphabet.txt" >}}  
```text
The type of hyphen: SEPARATOR
The type of U+1D400: LETTER
The type of U+1D41A: LETTER
Characters that are not separators: 2337
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/alphabet/alphabet/alphabet.txt)
{{< /tab >}}
{{< /tabs >}}

The following example demonstrates how to create another index with the same alphabet by importing the alphabet exported in the previous example.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating a new index in the specified folder
new_index = Index("./new-index", True)

# Setting the Separator type for all characters, so that only the imported types remain in the alphabet
new_index.dictionaries.alphabet.clear()

# Importing the alphabet from a file
new_index.dictionaries.alphabet.import_dictionary("./Alphabet.dat")
print("Characters that are not separators:", new_index.dictionaries.alphabet.count)

# Indexing documents
new_index.add("./documents")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/alphabet/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/alphabet/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/alphabet/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "alphabet-2.txt" >}}  
```text
Characters that are not separators: 2337
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/alphabet/alphabet_2/alphabet-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
