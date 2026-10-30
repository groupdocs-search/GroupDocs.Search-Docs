---
id: phrase-search
url: search/python-net/phrase-search
title: Phrase search
weight: 14
description: "This article defines phrase search and describes phrase search with wildcards in GroupDocs.Search."
keywords: Phrase search, phrase search definition, phrase search description
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains a phrase search definition as well as a description of phrase search with the GroupDocs.Search API.

## Phrase search definition

Phrase search is a type of search that allows users to search for documents containing an exact sentence or phrase rather than containing a set of keywords in random order.

## Phrase search in GroupDocs.Search

Phrase search allows you to search for an exact phrase and find a given sequence of words in the text of indexed documents. In text form, the following syntax is used to specify a phrase search query:

*   "word1 word2 word3 ..."

Quotation marks are required, for example, in Python:

`query = '"theory of relativity"'`

Please note that if stop words are used in a search query, the phrase will still be found; however, any words can be found in place of the stop words.

The following example demonstrates performing the phrase search with a query in text and object form.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search for the phrase 'theory of relativity' in text form
query1 = '"theory of relativity"'
result1 = index.search(query1)

# Search for the phrase 'theory of relativity' in object form
word1 = SearchQuery.create_word_query("theory")
word2 = SearchQuery.create_word_query("of")
word3 = SearchQuery.create_word_query("relativity")
query2 = SearchQuery.create_phrase_search_query(word1, word2, word3)
result2 = index.search(query2)

# Printing the documents found
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/phrase-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/phrase-search/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/phrase-search/documents/gravity-notes.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "phrase-search-in-groupdocs-search.txt" >}}  
```text
Documents found by the text query: 2
Documents found by the object query: 2
    einstein-biography.txt
    relativity-primer.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/phrase-search/phrase_search_in_groupdocs_search/phrase-search-in-groupdocs-search.txt)
{{< /tab >}}
{{< /tabs >}}

## Phrase search with wildcards

In addition to words, phrase search queries can also use two kinds of wildcards:

*   \* byte-number
*   \* byte-number ~~ byte-number

where byte-number is a number from 0 to 255.

The first pattern represents the exact number of unknown words in a phrase, for example \*2. The second pattern represents the range of the number of unknown words in a phrase, for example \*1~~2. In object form, these wildcards are created with the [create_wildcard_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_wildcard_query/) method; such a query can be used only as a part of a phrase search query.

Phrase search query with wildcards is flexible enough to be used instead of span term query (span query).

The following example demonstrates the use of wildcards in phrase search queries in text and object form.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search for the phrase with a wildcard in text form
query1 = '"theory *1~~2 relativity"'
result1 = index.search(query1)

# Search for the phrase with a wildcard in object form
word1 = SearchQuery.create_word_query("theory")
wildcard2 = SearchQuery.create_wildcard_query(1, 2)
word3 = SearchQuery.create_word_query("relativity")
query2 = SearchQuery.create_phrase_search_query(word1, wildcard2, word3)
result2 = index.search(query2)

# The search can find the following phrases:
# "theory of relativity"
# "theory of special relativity"

# Printing the phrases found
# (stop words and the words matched by a wildcard are reported as *)
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path))
    for field in document.found_fields:
        for sequence in field.term_sequences:
            print("       ", " ".join(sequence))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/phrase-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/phrase-search/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/phrase-search/documents/gravity-notes.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "phrase-search-with-wildcards.txt" >}}  
```text
Documents found by the text query: 2
Documents found by the object query: 2
    einstein-biography.txt
        theory * relativity
    relativity-primer.txt
        theory * relativity
        theory * * relativity
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/phrase-search/phrase_search_with_wildcards/phrase-search-with-wildcards.txt)
{{< /tab >}}
{{< /tabs >}}

Phrase search can be combined with other types of searches. The specification of search queries in text form is presented on the [Query language specification]({{< ref "search/python-net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}) page. A table of the possibility of nesting search queries in object form is presented on the [Nesting search queries in object form]({{< ref "search/python-net/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form.md" >}}) page.

The following example demonstrates the use of both wildcards representing words and characters in words.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery
from groupdocs.search.common import WordPattern

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search for the phrase in text form
query1 = '"Test?(0~1) *1~~2 relativity"'
result1 = index.search(query1)

# Search for the phrase in object form
pattern = WordPattern()
pattern.append_string("Test")
pattern.append_wildcard(0, 1)
word_pattern1 = SearchQuery.create_word_pattern_query(pattern)
wildcard2 = SearchQuery.create_wildcard_query(1, 2)
word3 = SearchQuery.create_word_query("relativity")
query2 = SearchQuery.create_phrase_search_query(word_pattern1, wildcard2, word3)
result2 = index.search(query2)

# The search can find the following phrases:
# "Test of special relativity"
# "Tests of special relativity"
# "Test of general relativity"
# "Tests of general relativity"

# Printing the phrases found
# (stop words and the words matched by a wildcard are reported as *)
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path))
    for field in document.found_fields:
        for sequence in field.term_sequences:
            print("       ", " ".join(sequence))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/phrase-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/phrase-search/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/phrase-search/documents/gravity-notes.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "phrase-search-with-wildcards-2.txt" >}}  
```text
Documents found by the text query: 1
Documents found by the object query: 1
    relativity-primer.txt
        test * * relativity
        tests * * relativity
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/phrase-search/phrase_search_with_wildcards_2/phrase-search-with-wildcards-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
