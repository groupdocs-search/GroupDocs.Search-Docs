---
id: search-for-text-containing-special-characters
url: search/python-net/search-for-text-containing-special-characters
title: Search for text containing special characters
weight: 20.5
description: "This article shows how to handle search queries if they contain special characters and separator characters."
keywords: Search for text containing special characters
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
If a search query must contain special characters that are used as ordinary characters and not as query syntax elements, these characters must be escaped.
The special characters are:
<pre>
( ) : " & | ! ^ ~ * ? \ <i>space</i>
</pre>
These characters can be escaped by adding a \\ sign to the left of the character. For example:  
\\& - escaping & character;  
\\~ - escaping ~ character;  
\\s - escaping space character.  
See [Search operation table]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-operation-table.md" >}}) for details on search query syntax.

However, if the escaped character is a separator, it will still not be found in the text, since separators are not indexed. As a result, words containing such characters will also not be found when searching for an exact match.

If the search query contains separator characters, words containing these characters will not be found, because during indexing these characters break the word into parts and are not indexed themselves.
Therefore, when performing a search, the query must be checked for the content of separator characters. Each word containing such characters should be broken into several smaller words and form a search phrase.

In the example below, the character types are set in the alphabet of a new index before documents are added. Since version 26.10, the alphabet can be changed only while the index contains no documents; otherwise, InvalidOperationException is thrown (RuntimeError in Python). For details, see the [Alphabet]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}) page.

The order of processing each word in a search query is as follows:
1. If separators are present in the word, they are replaced by spaces, and the word search is converted to a phrase search.
2. If special characters are present in the word, they are escaped.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.dictionaries import CharacterType

# Creating a new index in the specified folder (the existing index files are deleted)
index = Index("./index", True)

# Setting character types before indexing documents
index.dictionaries.alphabet.set_range(["&"], CharacterType.LETTER)
index.dictionaries.alphabet.set_range(["-"], CharacterType.SEPARATOR)

# Indexing documents from the specified folder
index.add("./documents")

# Defining a search query
word = "rock&roll-music"

# Replacing separators with the space characters
result = []
for character in word:
    character_type = index.dictionaries.alphabet.get_character_type(character)
    if character_type == CharacterType.SEPARATOR:
        result.append(" ")
    else:
        result.append(character)

# Escaping special characters
special_characters = "():\"&|!^~*?\\"
for i in range(len(result) - 1, -1, -1):
    if result[i] in special_characters:
        result.insert(i, "\\")

query = "".join(result)
if " " in query:
    query = "\"" + query + "\""

# Searching
search_result = index.search(query)

# Printing the query and the documents found
print("Query:", query)
print("Documents found:", search_result.document_count)
for document in search_result:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents/music-history.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-for-text-containing-special-characters/documents/music-history.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-text-containing-special-characters.txt" >}}  
```text
Query: "rock\&roll music"
Documents found: 1
    music-history.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/search-for-text-containing-special-characters/search_for_text_containing_special_characters/search-text-containing-special-characters.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
