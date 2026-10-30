---
id: tokenization-dictionary
url: search/python-net/tokenization-dictionary
title: Tokenization dictionary
weight: 9
description: "This article describes the tokenization dictionary of an index, which defines the words returned by a custom text segmenter that are indexed, in GroupDocs.Search for Python via .NET."
keywords: tokenization dictionary, custom text segmenter, word splitter, IWordSplitter, CJK, hieroglyphic languages
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The tokenization dictionary of an index is available through the [tokenization_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionaryrepository/tokenization_dictionary/) property of the [Index.dictionaries](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/dictionaries/) object. Like the [alphabet]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}), the tokenization dictionary is an instance of the [Alphabet](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/) class and contains the types of characters, but it is used for another purpose.

## Purpose of the tokenization dictionary

By default, the text of documents is divided into words by the built-in mechanism, which uses the alphabet: words are formed from continuous sequences of letters, and separators divide them. Some languages, in particular Chinese, Japanese and Korean, do not use spaces between words and require special text segmentation tools. For such documents, you can use an external text segmenter: write a word splitter, a class with the `split(text)` method of the [IWordSplitter](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/iwordsplitter/) interface that returns the words of the text (a function that takes the text also works), and assign it to the [word_splitter](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/fileindexingeventargs/word_splitter/) property of the [file_indexing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/file_indexing/) event arguments. For details, see the [Custom text segmenter]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/custom-text-segmenter.md" >}}) page.

When a document is indexed with a custom word splitter, the alphabet is not used to divide its text into words. The word splitter determines the boundaries of words, and the tokenization dictionary determines which of the returned words are indexed:

*   a word is indexed if at least one of its characters has a type other than [CharacterType](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/charactertype/).SEPARATOR in the tokenization dictionary;
*   a word that consists only of separators, for example, a sequence of spaces or punctuation marks returned by the word splitter, is not indexed.

As in the case of the built-in mechanism, words longer than 80 characters are not indexed, and stop words are not indexed if the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) setting is enabled.

## Default content

By default, the tokenization dictionary defines the following characters as letters (Unicode numbers are given):

*   all characters that are letters in the default alphabet: digits, the low line character, and Latin, Greek and Coptic, Cyrillic, Armenian and Arabic letters (see the [Character types]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/character-types.md" >}}) page);
*   CJK Radicals Supplement: 2E80-2EFF;
*   CJK Symbols and Punctuation: 3000-303F;
*   CJK Strokes: 31C0-31EF;
*   CJK Compatibility: 3300-33FF;
*   CJK Unified Ideographs Extension A: 3400-4DBF;
*   CJK Unified Ideographs: 4E00-9FFF;
*   CJK Compatibility Ideographs: F900-FAFF;
*   CJK Compatibility Forms: FE30-FE4F;
*   CJK Unified Ideographs Extensions B–H: 20000-2A6DF, 2A700-2B73F, 2B740-2B81F, 2B820-2CEAF, 2CEB0-2EBEF, 30000-3134F, 31350-323AF;
*   CJK Compatibility Ideographs Supplement: 2F800-2FA1F.

All other characters, including Japanese Hiragana and Katakana and Korean Hangul, are separators in the default tokenization dictionary. If a word splitter returns words of such characters, add the characters to the tokenization dictionary, as shown in the example below.

## Managing the tokenization dictionary

The tokenization dictionary provides the same methods as the alphabet:

*   The [get_character_type](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/get_character_type/) method returns the type of a character specified as a string of one character, also a character outside the Basic Multilingual Plane, or as a Unicode code point.
*   The [count](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/count/) property returns the number of characters of a type other than [CharacterType](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/charactertype/).SEPARATOR; iterating over the dictionary returns the Unicode code points of these characters.
*   The [set_range](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/set_range/) method sets the type of characters specified as one-character strings or as Unicode code points, characters outside the Basic Multilingual Plane included.
*   The [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/clear/) method sets the [CharacterType](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/charactertype/).SEPARATOR type for all characters. After that, no word returned by a custom word splitter is indexed until letters are defined again.
*   The [export_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/export_dictionary/) and [import_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/import_dictionary/) methods save the dictionary to a file and load it from a file. The imported types are set for the characters listed in the file; the types of other characters are not changed. Errors of exporting and importing are reported by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index instead of exceptions.

{{< alert style="info" >}}The tokenization dictionary defines which terms of documents are indexed. Therefore, the tokenization dictionary of an index can be changed only while the index contains no documents and is not performing an operation; otherwise, a method that changes the dictionary raises `RuntimeError`. A call that does not change the content of the dictionary is allowed. Configure the tokenization dictionary after creating a new index and before adding documents. See also the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

In a search network, the tokenization dictionary of a shard is obtained with the [get_tokenization_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_tokenization_dictionary/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class; see the [Managing dictionaries in network]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/managing-dictionaries-in-network.md" >}}) page.

## Example

The following example demonstrates a simple word splitter that divides text into words at white space characters. The words it returns may consist only of punctuation marks, for example, a standalone dash; such words are not indexed, because the tokenization dictionary defines all their characters as separators.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
# A simple word splitter that divides text into words at white space characters
class WhiteSpaceWordSplitter:
    def split(self, text):
        return text.split()
```
{{< /tab >}}
{{< /tabs >}}

The following example demonstrates how to configure the tokenization dictionary of a new index and use the word splitter from the previous example to index text documents. The search at the end finds a Hiragana word, which the default tokenization dictionary would not index.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.dictionaries import CharacterType


# A simple word splitter that divides text into words at white space characters
class WhiteSpaceWordSplitter:
    def split(self, text):
        return text.split()


# Creating a new index in the specified folder;
# the tokenization dictionary can be changed only while the index contains no documents
index = Index("./index", True)

tokenization_dictionary = index.dictionaries.tokenization_dictionary

# Getting the type of a Hiragana character, which is a separator by default
print("The type of U+3042:", tokenization_dictionary.get_character_type("あ").name)

# Setting the Letter type for Hiragana and Katakana characters
code_points = list(range(0x3040, 0x3100))
tokenization_dictionary.set_range(code_points, CharacterType.LETTER)

# Getting the number of characters that are not separators
print("Characters that are not separators:", tokenization_dictionary.count)

# Exporting the tokenization dictionary to a file, for example, to use it in other indexes
tokenization_dictionary.export_dictionary("./TokenizationDictionary.dat")

# Using the custom word splitter for text documents
word_splitter = WhiteSpaceWordSplitter()


def on_file_indexing(sender, args):
    if args.document_full_path.lower().endswith(".txt"):
        args.word_splitter = word_splitter


index.events.file_indexing += on_file_indexing

# Indexing documents; after that, the tokenization dictionary of the index cannot be changed
index.add("./documents")

# Searching for a Hiragana word
result = index.search("ひらがな")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary/documents/einstein-notes.txt), [documents/japanese.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary/documents/japanese.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "tokenization-dictionary-2.txt" >}}  
```text
The type of U+3042: SEPARATOR
Characters that are not separators: 101197
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary/tokenization_dictionary_2/tokenization-dictionary-2.txt)
{{< /tab >}}
{{< /tabs >}}

The following example demonstrates how to create another index with the same tokenization dictionary by importing the dictionary exported in the previous example.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating a new index in the specified folder
new_index = Index("./new-index", True)

# Setting the Separator type for all characters, so that only the imported types remain in the dictionary
new_index.dictionaries.tokenization_dictionary.clear()

# Importing the tokenization dictionary from a file
new_index.dictionaries.tokenization_dictionary.import_dictionary("./TokenizationDictionary.dat")
print("Characters that are not separators:", new_index.dictionaries.tokenization_dictionary.count)
```
{{< /tab >}}
{{< tab "tokenization-dictionary-3.txt" >}}  
```text
Characters that are not separators: 101197
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary/tokenization_dictionary_3/tokenization-dictionary-3.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
