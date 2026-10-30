---
id: character-replacement-during-indexing
url: search/python-net/character-replacement-during-indexing
title: Character replacement during indexing
weight: 1
description: "Character replacement during indexing can be used, for example, to convert all text to lowercase characters or to remove diacritics from text."
keywords: character replacement during indexing, character replacements, convert text to lowercase, remove diacritics, use_character_replacements, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Character replacement during indexing can be used, for example, to convert all text to lowercase characters or to remove diacritics from text. Such replacements can reduce the size of an index on disk if the case of characters or diacritics are not significant. Character replacements are performed during indexing only if the [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/) property of the index settings is `True`; the default value is `False`. See also [Character replacements]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/character-replacements.md" >}}) page in the [Managing dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

Since version 26.10, the character replacement dictionary and the [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/) setting can be changed only while the index contains no documents and is not performing an operation; otherwise, `RuntimeError` is raised. Therefore, configure them after creating a new index and before adding the first document, as the example below does. To use other character replacements with documents that are already indexed, create a new index and index the documents again. See the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page for details.

The example below demonstrates how to configure and use character replacements during indexing. A character that is not in the dictionary is not replaced, so the example adds pairs only for the characters whose lowercase form differs. The pairs are added with the [add_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementdictionary/add_range/) method, which takes a list of [CharacterReplacementPair](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/characterreplacementpair/) objects.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings
from groupdocs.search.dictionaries import CharacterReplacementPair

# Enabling character replacements in the index settings
settings = IndexSettings()
settings.use_character_replacements = True

# Creating a new index in the specified folder
# The character replacements can be changed only while the index contains no documents
index = Index("./index", settings, True)

# Configuring character replacements
# Deleting all existing character replacements from the dictionary
index.dictionaries.character_replacements.clear()
# Creating new character replacements: each character is replaced with its lowercase form
character_replacements = []
for code_point in range(0x10000):
    character = chr(code_point)
    replacement = character.lower()
    # A character that does not change needs no replacement
    if replacement != character and len(replacement) == 1:
        character_replacements.append(CharacterReplacementPair(character, replacement))
# Adding character replacements to the dictionary
index.dictionaries.character_replacements.add_range(character_replacements)

# Indexing documents from the specified folder
index.add("./documents")

# Searching in the index
# Case-sensitive search is no longer possible for this index, since all characters are lowercase
# By default, case-insensitive search is performed
result = index.search("Einstein")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-replacement-during-indexing/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-replacement-during-indexing/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "character-replacement-during-indexing.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/character-replacement-during-indexing/character_replacement_during_indexing/character-replacement-during-indexing.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
