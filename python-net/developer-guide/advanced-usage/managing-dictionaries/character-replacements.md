---
id: character-replacements
url: search/python-net/character-replacements
title: Character replacements
weight: 3
description: "This article describes the Python API for managing the character replacement dictionary of an index."
keywords: character replacements, character replacement dictionary
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
An instance of the [CharacterReplacementDictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementdictionary/) class contains all the character replacements defined in an index. Character replacements are applied during indexing only if the [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/) index setting is enabled. For detailed information on character replacement, see the [Character replacement during indexing]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/character-replacement-during-indexing.md" >}}) page.

{{< alert style="info" >}}Character replacements define how the text of documents is divided into terms. Therefore, the character replacement dictionary and the [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/) setting of an index can be changed only while the index contains no documents and is not performing an operation; otherwise, a change raises `RuntimeError`. A call that does not change the content of the dictionary is allowed. Configure character replacements after creating a new index and before adding documents. See also the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

The [count](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementdictionary/count/) property allows you to get the number of character replacements defined in the dictionary.

To add character replacements to the dictionary, use the [add_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementdictionary/add_range/) method. It takes a list of [CharacterReplacementPair](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementpair/) objects, or of `(character, replacement)` tuples. A pair can be created from two one-character strings or from two Unicode code points (`int` values); the latter allows you to define replacements for characters outside the Basic Multilingual Plane. A replacement added for a character that already has one replaces the previous replacement. A pair whose replacement is the same as the character removes the replacement of the character.

To remove character replacements from the dictionary, the [remove_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementdictionary/remove_range/) method is used. The characters can be specified as one-character strings or as Unicode code points.

The [contains](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementdictionary/contains/) method is used to determine if the dictionary contains a replacement for the specified character. The character is specified as a Unicode code point, for example `ord("-")`.

To get a replacement for the specified character, use the [get_replacement](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementdictionary/get_replacement/) method, which also takes the Unicode code point of the character. The method returns the Unicode code point of the replacement, or the specified character itself if the dictionary contains no replacement for it.

To remove all replacements from the dictionary, use the [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementdictionary/clear/) method.

To export all replacements to a file, use the [export_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/export_dictionary/) method.

To import character replacements from a file, use the [import_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/import_dictionary/) method. Errors of exporting and importing are reported by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index instead of exceptions.

The following example demonstrates the use of the character replacement dictionary methods.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings
from groupdocs.search.dictionaries import CharacterReplacementPair

# Creating an index settings instance
settings = IndexSettings()
settings.use_character_replacements = True  # Enabling character replacements during indexing

# Creating a new index in the specified folder; the dictionary can be changed only while the index contains no documents
index = Index("./index", settings, True)

replacements = index.dictionaries.character_replacements

if replacements.contains(ord("-")):
    # Getting the replacement for the hyphen character
    replacement = replacements.get_replacement(ord("-"))
    print("The replacement for hyphen is " + chr(replacement))

    # Deleting the hyphen character replacement from the dictionary
    replacements.remove_range(["-"])

if replacements.count > 0:
    # Deleting all character replacements from the dictionary
    replacements.clear()

# Adding character replacements to the dictionary
pairs = [
    CharacterReplacementPair("Ä", "a"),
    CharacterReplacementPair("ä", "a"),
    CharacterReplacementPair(0x1D400, 0x0061),  # Mathematical bold capital A is replaced with the small letter a
]
replacements.add_range(pairs)

# Exporting character replacements to a file
replacements.export_dictionary("./CharacterReplacements.dat")

# Importing character replacements from a file
replacements.import_dictionary("./CharacterReplacements.dat")
print("Character replacements:", replacements.count)
print("The replacement for Ä is " + chr(replacements.get_replacement(ord("Ä"))))

# Indexing documents; after that, the character replacements of the index cannot be changed
index.add("./documents")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/character-replacements/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/character-replacements/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/character-replacements/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "character-replacements.txt" >}}  
```text
Character replacements: 3
The replacement for Ä is a
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/character-replacements/character_replacements/character-replacements.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
