---
id: character-types
url: search/python-net/character-types
title: Character types
weight: 2
description: "This page contains descriptions of all character types. Character types differ in how characters of these types are indexed."
keywords: character types, alphabet, separator, letter, blended characters, separate word, CharacterType, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains descriptions of all character types. Character types differ in how characters of these types are indexed. The type of each character is specified in the alphabet of an index, which is described by the [Alphabet](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/) class. Character types are represented by the [CharacterType](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/charactertype/) enumeration: **SEPARATOR**, **LETTER**, **BLENDED** and **SEPARATE_WORD**. For more information on managing the alphabet, see the [Alphabet]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}) page in the [Managing dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The alphabet defines how the text of documents is divided into words, so it can be changed only while the index contains no documents and is not performing an operation. Otherwise, a method that changes the alphabet raises `RuntimeError`. Therefore, configure the alphabet after creating a new index and before adding the first document, as the examples below do. To use another alphabet with documents that are already indexed, create a new index and index the documents again. See the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page for details.

## Regular characters

Regular characters are separators and letters. During indexing, separators are used to separate words from each other, and words are formed from continuous sequences of letters.

The type of each particular character can be specified in the alphabet. By default, the following characters are defined as letters (Unicode code points are given):

*   Digits: 0030-0039;
*   Latin capital letters: 0041-005A;
*   Low line: 005F;
*   Latin small letters: 0061-007A;
*   Latin letters: 00C0-00D6, 00D8-00F6, 00F8-00FF, 0100-017F, 0180-024F, 0250-02AF;
*   Greek and Coptic letters: 0370-0373, 0376-0377, 037F, 0386, 0388-038A, 038C, 038E-03A1, 03A3-03FC;
*   Cyrillic letters: 0400-0481, 048A-04FF, 0500-052F;
*   Armenian letters: 0531-0556, 0561-0587;
*   Arabic letters: 0610-061A, 0620-0669, 066E-06D3, 06D5-06DC, 06DF-06E8, 06EA-06FC, 06FF, 0750-077F, 0870-0887, 0889-088E, 0898-089F, 08A0-08E1, 08E3-08FF, FB50-FBB1, FBD3-FD3D, FD50-FD8F, FD92-FDC7, FDF0-FDFB, FE70-FE74, FE76-FEFC, 10EFD-10EFF, as well as the letters of the Arabic Mathematical Alphabetic Symbols block (1EE00-1EEBB).

The other characters are defined as separators in the alphabet.

The following example demonstrates how to specify only digits, the low line character, and English letters as letters in the alphabet.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.dictionaries import CharacterType

# Creating a new index in the specified folder
# The alphabet can be changed only while the index contains no documents
index = Index("./index", True)

# Configuring the alphabet
# Setting the separator type for all characters in the alphabet
index.dictionaries.alphabet.clear()
# Creating a list of letter characters
characters = []
for code_point in range(0x0030, 0x0039 + 1):
    characters.append(chr(code_point))  # Digits
for code_point in range(0x0041, 0x005A + 1):
    characters.append(chr(code_point))  # Latin capital letters
characters.append(chr(0x005F))  # Low line
for code_point in range(0x0061, 0x007A + 1):
    characters.append(chr(code_point))  # Latin small letters
# Setting the type of characters in the alphabet
index.dictionaries.alphabet.set_range(characters, CharacterType.LETTER)

# Indexing documents from the specified folder
index.add("./documents")

# Searching in the index
result = index.search("Einstein")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/Newton.txt), [documents/EarlOfMinto.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/EarlOfMinto.txt), [documents/Chinese.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/Chinese.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "regular-characters.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/character-types/regular_characters/regular-characters.txt)
{{< /tab >}}
{{< /tabs >}}

The [set_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/set_range/) method also takes a list of Unicode code points (integers), which allows you to specify characters outside the Basic Multilingual Plane, for example `index.dictionaries.alphabet.set_range([0x1D538], CharacterType.LETTER)`. A list of one-character strings works too, and such a string may hold a character outside the Basic Multilingual Plane: `set_range(["\U0001D538"], CharacterType.LETTER)`.

## Blended characters

Blended characters are special characters that are indexed both as separators and as letters. This type of character can be useful, for example, for indexing hyphens. In this case, parts of a compound word containing a hyphen will be indexed both as a single word with a hyphen and individually without a hyphen. An example of using blended characters is presented below.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.dictionaries import CharacterType

# Creating a new index in the specified folder
index = Index("./index", True)

# Setting hyphen character type to blended
index.dictionaries.alphabet.set_range(["-"], CharacterType.BLENDED)

# Indexing documents from the specified folder
index.add("./documents")

# Searching in the index
result1 = index.search("Elliot-Murray-Kynynmound")
result2 = index.search("Elliot")
result3 = index.search("Murray")
result4 = index.search("Kynynmound")
for query, result in [("Elliot-Murray-Kynynmound", result1), ("Elliot", result2),
                      ("Murray", result3), ("Kynynmound", result4)]:
    print(query + ":", result.document_count, "document(s)")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/Newton.txt), [documents/EarlOfMinto.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/EarlOfMinto.txt), [documents/Chinese.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/Chinese.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "blended-characters.txt" >}}  
```text
Elliot-Murray-Kynynmound: 1 document(s)
Elliot: 1 document(s)
Murray: 1 document(s)
Kynynmound: 1 document(s)
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/character-types/blended_characters/blended-characters.txt)
{{< /tab >}}
{{< /tabs >}}

A sequence of blended characters without letters, for example a standalone hyphen, also forms a term. By default, such terms are not indexed, because the [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) index setting is enabled. To keep them in the index, disable this setting when creating the index (see [Search index settings]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}})).

## Indexing each character as a whole word

Another special type of character is a character indexed as a separate word, the **SEPARATE_WORD** value of the [CharacterType](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/charactertype/) enumeration. This type of character is designed to work with hieroglyphic languages and allows you to index each such character in the text as a separate word, regardless of the presence of separators. By default, no characters of this type are defined in the alphabet. As an alternative for such languages, you can split text into words with an external library, see the [Custom text segmenter]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/custom-text-segmenter.md" >}}) page.

The following example demonstrates how to index each CJK unified ideograph as a separate word.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.dictionaries import CharacterType

# Creating a new index in the specified folder
index = Index("./index", True)

# Setting the separate word type for the CJK unified ideographs
characters = [chr(code_point) for code_point in range(0x4E00, 0x9FFF + 1)]
index.dictionaries.alphabet.set_range(characters, CharacterType.SEPARATE_WORD)

# Indexing documents from the specified folder
index.add("./documents")

# Searching in the index for a single character
result = index.search("书")
print("Documents found:", result.document_count)
print("Occurrences found:", result.occurrence_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/Newton.txt), [documents/EarlOfMinto.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/EarlOfMinto.txt), [documents/Chinese.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/character-types/documents/Chinese.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-each-character-as-a-whole-word.txt" >}}  
```text
Documents found: 1
Occurrences found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/character-types/indexing_each_character_as_a_whole_word/indexing-each-character-as-a-whole-word.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
