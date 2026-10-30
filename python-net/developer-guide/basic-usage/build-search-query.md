---
id: build-search-query
url: search/python-net/build-search-query
title: Build search query
weight: 2
description: "This article describes how to build text search queries of various types in Python: simple, wildcard, faceted, numeric range, date range, regular expression, boolean and phrase queries."
keywords: search queries, building text search queries, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
structuredData:
  showOrganization: True
  application:
    name: Document Search
    description: Index documents and search them natively with high performance using Python language and GroupDocs.Search for Python via .NET
    productCode: search
    productPlatform: python-net
  howTo:
    name: How to build search queries in Python
    description: Learn how to search an index with text queries of various types in Python step by step
    steps:
      - name: Create an index and add documents
        text: Create an object of the Index class with the path to the index folder and call its add method with the path to a folder of documents.
      - name: Write a search query
        text: Write the query as a string; its syntax selects the type of search, for example a wildcard, a numeric range, a regular expression, a boolean or a phrase query.
      - name: Search
        text: Pass the query, and optionally a SearchOptions object, to the search method of the index. The method returns a SearchResult object.
---
This page contains information about building text search queries of various types. More examples on building search queries are provided on the page [Search operation table]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-operation-table.md" >}}). A detailed specification of search query language is presented on the page [Query language specification]({{< ref "search/python-net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}).

In addition to text search queries, there is also the ability to create queries in an object form. Search query forms are described on the page [Queries in text and object form]({{< ref "search/python-net/developer-guide/advanced-usage/searching/queries-in-text-and-object-form.md" >}}).

You can also use search options to customize your search queries. Detailed information on search options is provided on the page [Search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-options.md" >}}).

The examples on this page search in an index containing indexed documents. Such an index can be created as follows; each example below starts with these lines, so it can be run on its own.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")  # Creating an index in the specified folder
index.add("./documents")  # Indexing documents from the specified folder
print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "build-search-query.txt" >}}  
```text
Indexed documents: 4
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/build_search_query/build-search-query.txt)
{{< /tab >}}
{{< /tabs >}}

## Simple search query

The first type of search query is a simple search query. Queries of this type are used to search for a single word in the text of a document. The example is presented below.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = "Einstein"
result = index.search(query)  # Search in the index
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "simple-search-query.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/simple_search_query/simple-search-query.txt)
{{< /tab >}}
{{< /tabs >}}

## Wildcard search query

A more complex type of query is a wildcard search query. There are 2 types of wildcard characters in a word: a one character wildcard and a character group wildcard. Detailed information about wildcards in a word is provided on the page [Wildcard search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/wildcard-search.md" >}}). The example below demonstrates use of the one character wildcard.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = "?ffect"
result = index.search(query)  # Search for words 'affect', 'effect', etc.

# Printing the found words
for i in range(result.document_count):
    document = result.get_found_document(i)
    print(document.document_info.file_path, document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "wildcard-search-query.txt" >}}  
```text
documents\einstein.txt ['affect']
documents\nobel-prize-1921.txt ['effect']
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/wildcard_search_query/wildcard-search-query.txt)
{{< /tab >}}
{{< /tabs >}}

The character group wildcard allows you to specify a range of unknown characters in the form ?(N~M), where N and M are in the range from 0 to 255. The next example demonstrates use of the character group wildcard.

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = "princip?(2~4)"
result = index.search(query)  # Search for words 'principal', 'principle', 'principles', 'principally', etc.

# Printing the found words
for i in range(result.document_count):
    document = result.get_found_document(i)
    print(document.document_info.file_path, document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "wildcard-search-query-2.txt" >}}  
```text
documents\einstein.txt ['principle', 'principles']
documents\nobel-prize-1921.txt ['principal']
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/wildcard_search_query_2/wildcard-search-query-2.txt)
{{< /tab >}}
{{< /tabs >}}

## Faceted search query

The next type of query - faceted search query - allows you to specify the document field for the search. Detailed information about faceted search is presented on the page [Faceted search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/faceted-search.md" >}}). The following example shows how to search only in 'Content' field.

{{< tabs "example5" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = "Content: Einstein"
result = index.search(query)  # Search for word 'Einstein' only in 'Content' field
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "faceted-search-query.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/faceted_search_query/faceted-search-query.txt)
{{< /tab >}}
{{< /tabs >}}

## Numeric range search query

The following type of query allows you to specify a range of numbers to search - numeric range search query. Detailed information about numeric range search is presented on the page [Numeric range search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/numeric-range-search.md" >}}). The example below demonstrates how to search for numbers in the range from 2000 to 3000. The numbers are searched in all fields of a document, including the creation and modification dates of the file.

{{< tabs "example6" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = "2000 ~~ 3000"
result = index.search(query)  # Search for numbers from 2000 to 3000

# Printing the found numbers
for i in range(result.document_count):
    document = result.get_found_document(i)
    print(document.document_info.file_path, document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "numeric-range-search-query.txt" >}}  
```text
documents\einstein.txt ['2026']
documents\nobel-prize-1921.txt ['2026', '2001', '2500']
documents\relativity.txt ['2026']
documents\time.txt ['2026']
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/numeric_range_search_query/numeric-range-search-query.txt)
{{< /tab >}}
{{< /tabs >}}

## Date range search query

Search for dates from a particular range is performed using a date range search query. Details on the date range search are presented on the page [Date range search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/date-range-search.md" >}}). The example demonstrates how to search for dates in the format 'MM/dd/yyyy' in the range from 01/01/2000 to 06/15/2001.

{{< tabs "example7" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import DateFormat, DateFormatElement, SearchOptions

index = Index("./index")
index.add("./documents")

options = SearchOptions()  # Creating a search options object
options.date_formats.clear()  # Removing default date formats

# Creating a date format pattern 'MM/dd/yyyy'
elements = [
    DateFormatElement.MONTH_TWO_DIGITS,
    DateFormatElement.DATE_SEPARATOR,
    DateFormatElement.DAY_OF_MONTH_TWO_DIGITS,
    DateFormatElement.DATE_SEPARATOR,
    DateFormatElement.YEAR_FOUR_DIGITS,
]
date_format = DateFormat(elements, "/")

options.date_formats.add(date_format)  # Adding the date format pattern to the date format collection
query = "daterange(2000-01-01 ~~ 2001-06-15)"  # Dates in the search query are always specified in the format 'yyyy-MM-dd'

result = index.search(query, options)  # Search in index
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "date-range-search-query.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/date_range_search_query/date-range-search-query.txt)
{{< /tab >}}
{{< /tabs >}}

## Regular expression search query

The next type of query is a regular expression search query. Detailed information about queries of this type is presented on the page [Regular expression search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/regular-expression-search.md" >}}). The following example demonstrates the use of regular expressions to search in an index.

{{< tabs "example8" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = r"^(.)\1{2,}"  # The caret character at the beginning indicates that this is a regular expression search query
result = index.search(query)  # Search for three or more identical characters in a row

# Printing the found words
for i in range(result.document_count):
    document = result.get_found_document(i)
    print(document.document_info.file_path, document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "regular-expression-search-query.txt" >}}  
```text
documents\nobel-prize-1921.txt ['www']
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/regular_expression_search_query/regular-expression-search-query.txt)
{{< /tab >}}
{{< /tabs >}}

## Boolean search query

The next type of search query is a boolean search query. Queries of this type allow you to use the logical conditions for the presence or absence of particular words in documents. The boolean operators AND, OR, NOT can be used in queries of this type. Detailed information about this type of search is presented on the page [Boolean search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/boolean-search.md" >}}). The following example demonstrates how to search for documents containing the word 'Einstein' but not containing the word 'relativity'.

{{< tabs "example9" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = "Einstein AND NOT relativity"
result = index.search(query)
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "boolean-search-query.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/boolean_search_query/boolean-search-query.txt)
{{< /tab >}}
{{< /tabs >}}

Boolean search queries can be combined using parentheses and other types of queries. For example, the following query searches for all documents containing words that start with 'relativ' in the file name or that contain both the words 'Einstein' and 'Albert' in the document content.

{{< tabs "example10" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = "FileName: relativ?(1~3) OR Content: (Einstein AND Albert)"
result = index.search(query)  # Search for documents whose file names contain 'relative', 'relativity', etc., or documents containing both 'Einstein' and 'Albert' in the content
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "boolean-search-query-2.txt" >}}  
```text
Documents found: 3
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/boolean_search_query_2/boolean-search-query-2.txt)
{{< /tab >}}
{{< /tabs >}}

## Phrase search query

Another type of query - a phrase search query - allows you to search entire phrases in documents. Detailed information on the phrase search is on the page [Phrase search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/phrase-search.md" >}}). An example of the phrase search is presented below.

{{< tabs "example11" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = '"theory of special relativity"'
result = index.search(query)  # Search for the phrase 'theory of special relativity'
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-search-query/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "phrase-search-query.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-search-query/phrase_search_query/phrase-search-query.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### Advanced usage topics

To learn more about search features and get familiar how to enhance your search solution, please refer to the [advanced usage section]({{< ref "search/python-net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured Python library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
