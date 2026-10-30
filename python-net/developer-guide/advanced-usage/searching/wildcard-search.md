---
id: wildcard-search
url: search/python-net/wildcard-search
title: Wildcard search
weight: 29
description: "This article describes wildcard search, which allows you to search for words with unknown letters or ranges of letters."
keywords: Wildcard search
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Wildcard search allows you to search for words with unknown letters or ranges of letters.

In a search query in text form, there are 2 forms of wildcard characters:

*   ? for a single character;
*   ?(*n*~*m*) for a group of characters, where *n* and *m* are numbers from 0 to 255, and *n* <= *m*.

Wildcard search is similar to [regular expression search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/regular-expression-search.md" >}}), but it works significantly faster when there are fewer groups of wildcard characters and they are closer to the end of a search query.

Wildcard search is flexible enough to be used for prefix queries, since a prefix query is a special case of a wildcard query. Since version 26.10, wildcard search uses as many threads as there are processor cores by default.

Examples of wildcard search with queries in text form are presented below.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search in the index
result1 = index.search("m?rry")  # Search for 'merry', 'marry', etc.
result2 = index.search("card?(1~6)")  # Search for 'cardiff', 'cardinal', 'cardio', 'cards', etc.

# Printing the words found
for query, result in (("m?rry", result1), ("card?(1~6)", result2)):
    print(query, "- documents found:", result.document_count)
    for document in result:
        print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/word-games.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/wildcard-search/documents/word-games.txt), [documents/zoo-guide.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/wildcard-search/documents/zoo-guide.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "wildcard-search.txt" >}}  
```text
m?rry - documents found: 1
    word-games.txt ['marry', 'merry']
card?(1~6) - documents found: 1
    word-games.txt ['cardiff', 'cardinal', 'cardio', 'cards']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/wildcard-search/wildcard_search/wildcard-search.txt)
{{< /tab >}}
{{< /tabs >}}

To build a query for the wildcard search in object form, use the [WordPattern](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/wordpattern/) class. This class contains the following methods for adding known parts of a word and wildcards to a pattern:

*   [append_string](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/wordpattern/append_string/) appends a string of known characters.
*   [append_character](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/wordpattern/append_character/) appends a known character.
*   [append_one_character_wildcard](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/wordpattern/append_one_character_wildcard/) appends a wildcard for exactly one character (? in text form).
*   [append_zero_or_one_character_wildcard](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/wordpattern/append_zero_or_one_character_wildcard/) appends a wildcard for zero or one character (?(0~1) in text form).
*   [append_zero_or_more_characters_wildcard](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/wordpattern/append_zero_or_more_characters_wildcard/) appends a wildcard for zero to 255 characters (?(0~255) in text form).
*   [append_one_or_more_characters_wildcard](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/wordpattern/append_one_or_more_characters_wildcard/) appends a wildcard for one to 255 characters (?(1~255) in text form).
*   [append_wildcard](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/wordpattern/append_wildcard/) appends a wildcard for the specified minimum and maximum numbers of characters (?(min~max) in text form). Both values must be in the range from 0 to 255, and the minimum must not be greater than the maximum; otherwise, ArgumentOutOfRangeException is thrown (ValueError in Python).

The known characters are converted to lower case, since words are stored in the index in lower case. An example of constructing a query in object form is presented below.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery
from groupdocs.search.common import WordPattern

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search with pattern "m?rry"
# Search for 'merry', 'marry', etc.
pattern1 = WordPattern()
pattern1.append_string("m")
pattern1.append_one_character_wildcard()
pattern1.append_string("rry")
query1 = SearchQuery.create_word_pattern_query(pattern1)
result1 = index.search(query1)

# Search with pattern "card?(1~6)"
# Search for 'cardiff', 'cardinal', 'cardio', 'cards', etc.
pattern2 = WordPattern()
pattern2.append_string("card")
pattern2.append_wildcard(1, 6)
query2 = SearchQuery.create_word_pattern_query(pattern2)
result2 = index.search(query2)

# Printing the words found
for pattern, result in ((pattern1, result1), (pattern2, result2)):
    print(pattern.to_string(), "- documents found:", result.document_count)
    for document in result:
        print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/word-games.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/wildcard-search/documents/word-games.txt), [documents/zoo-guide.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/wildcard-search/documents/zoo-guide.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "wildcard-search-2.txt" >}}  
```text
m?rry - documents found: 1
    word-games.txt ['marry', 'merry']
card?(1~6) - documents found: 1
    word-games.txt ['cardiff', 'cardinal', 'cardio', 'cards']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/wildcard-search/wildcard_search_2/wildcard-search-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
