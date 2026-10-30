---
id: boolean-search
url: search/python-net/boolean-search
title: Boolean search
weight: 1
description: "This article defines boolean search and describes the boolean operators AND, OR, NOT with boolean query examples."
keywords: boolean search definition, boolean search, boolean query, boolean query examples 
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains the boolean search definition, a description of all boolean operators used for boolean search, and boolean query examples.

## Boolean search terms

Boolean search is a type of search that allows you to combine queries with boolean operators AND, OR, NOT to create a boolean query and further obtain more relevant results. For example, a boolean query might be "Albert AND Einstein". This will limit the search results to only documents that contain both words.

## Operator AND

The AND operator allows you to find only those documents that are found for each nested search query separately. The following example demonstrates the use of the AND operator in text and object form queries. The queries search for documents whose text contains both words "theory" and "relativity".

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search with text query
result1 = index.search("theory AND relativity")

# Search with object query
word_query1 = SearchQuery.create_word_query("theory")
word_query2 = SearchQuery.create_word_query("relativity")
and_query = SearchQuery.create_and_query(word_query1, word_query2)
result2 = index.search(and_query)

# Printing the documents found
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/gravity-notes.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "operator-and.txt" >}}  
```text
Documents found by the text query: 2
Documents found by the object query: 2
    einstein-biography.txt
    relativity-primer.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/boolean-search/operator_and/operator-and.txt)
{{< /tab >}}
{{< /tabs >}}

## Operator OR

The OR operator allows you to find all the documents that are found for at least one nested search query. The example demonstrates the use of the OR operator in text and object form queries. The queries search for documents whose text contains at least one of the words "Einstein" and "relativity".

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search with text query
result1 = index.search("Einstein OR relativity")

# Search with object query
word_query1 = SearchQuery.create_word_query("Einstein")
word_query2 = SearchQuery.create_word_query("relativity")
or_query = SearchQuery.create_or_query(word_query1, word_query2)
result2 = index.search(or_query)

# Printing the documents found
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/gravity-notes.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "operator-or.txt" >}}  
```text
Documents found by the text query: 4
Documents found by the object query: 4
    einstein-biography.txt ['einstein', 'relativity']
    gravity-notes.txt ['relativity']
    nobel-prize.txt ['einstein']
    relativity-primer.txt ['relativity']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/boolean-search/operator_or/operator-or.txt)
{{< /tab >}}
{{< /tabs >}}

## Operator NOT

The NOT operator allows you to invert the result of a nested search query and find all documents in which the nested search query finds no occurrences. The example demonstrates the use of the NOT operator in text and object form queries. The queries search for documents whose text contains the word "relativity" but does not contain the word "Einstein".

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search with text query
result1 = index.search("relativity AND NOT Einstein")

# Search with object query
word_query1 = SearchQuery.create_word_query("relativity")
word_query2 = SearchQuery.create_word_query("Einstein")
not_query = SearchQuery.create_not_query(word_query2)
and_query = SearchQuery.create_and_query(word_query1, not_query)
result2 = index.search(and_query)

# Printing the documents found
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/gravity-notes.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "operator-not.txt" >}}  
```text
Documents found by the text query: 2
Documents found by the object query: 2
    gravity-notes.txt
    relativity-primer.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/boolean-search/operator_not/operator-not.txt)
{{< /tab >}}
{{< /tabs >}}

## Complex queries

Boolean search operators can be combined using parentheses. The example below shows how to use parentheses to construct complex boolean search queries. In the example the query is presented in text and object form and searches for documents containing the words "theory" and "relativity" and not containing the words "Einstein" and "Albert".

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search with text query
result1 = index.search("(theory AND relativity) AND NOT (Einstein OR Albert)")

# Search with object query
theory_word_query = SearchQuery.create_word_query("theory")
relativity_word_query = SearchQuery.create_word_query("relativity")
and_query = SearchQuery.create_and_query(theory_word_query, relativity_word_query)

einstein_word_query = SearchQuery.create_word_query("Einstein")
albert_word_query = SearchQuery.create_word_query("Albert")
or_query = SearchQuery.create_or_query(einstein_word_query, albert_word_query)
not_query = SearchQuery.create_not_query(or_query)

root_query = SearchQuery.create_and_query(and_query, not_query)
result2 = index.search(root_query)

# Printing the documents found
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/gravity-notes.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "complex-queries.txt" >}}  
```text
Documents found by the text query: 1
Documents found by the object query: 1
    relativity-primer.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/boolean-search/complex_queries/complex-queries.txt)
{{< /tab >}}
{{< /tabs >}}

In object form, the [create_and_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_and_query/) and [create_or_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_or_query/) methods also take two or more child queries, as separate arguments or as one list. For example, the following query finds documents containing all three words.

{{< tabs "example5" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search with object query combining three words with the AND operator
and_query = SearchQuery.create_and_query(
    SearchQuery.create_word_query("theory"),
    SearchQuery.create_word_query("special"),
    SearchQuery.create_word_query("relativity"))
result = index.search(and_query)

# Printing the documents found
print("Query:", and_query.to_string())
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/relativity-primer.txt), [documents/gravity-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/gravity-notes.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/boolean-search/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "complex-queries-2.txt" >}}  
```text
Query: ((theory AND special) AND relativity)
Documents found: 1
    relativity-primer.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/boolean-search/complex_queries_2/complex-queries-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
