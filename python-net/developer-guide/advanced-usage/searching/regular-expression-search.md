---
id: regular-expression-search
url: search/python-net/regular-expression-search
title: Regular expression search
weight: 17
description: "This article describes regular expression (RegEx) search queries, which are universal and very flexible but can be slow in large indexes."
keywords: Regular expression search
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Regular expression (RegEx) search queries are universal and very flexible, but at the same time, in large indexes, their performance can be low, because the regular expression is matched against the words of the index. Another limitation of regular expressions is that search with a pattern is done for each word separately, and not the entire text of a document field. Words are stored in the index in lower case, therefore regular expression search is always case-insensitive.

Queries of this type in text form always start with the caret character, and if the regular expression itself also starts with the caret character, then there will be two caret characters at the beginning of the query.

A regex search query in text form cannot be combined with other types of queries. See the [Query language specification]({{< ref "search/python-net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}) page. However, in object form, regular expressions can be combined with other queries. See the [Nesting search queries in object form]({{< ref "search/python-net/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form.md" >}}) page.

The regular expression is evaluated by .NET (System.Text.RegularExpressions), whose syntax matches Python's re module for common patterns.

The following example demonstrates the regex search in text and object forms. The expression is matched against the words of every field of a document, including the creation and modification dates of the file, so a time such as 18:00 adds the word "00" to the words found.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search with a regular expression in text form
query1 = "^^(.)\\1{1,}"  # The first caret character at the beginning indicates that this is a regular expression search query
result1 = index.search(query1)  # Search for two or more identical characters at the beginning of a word

# Search with a regular expression in object form
query2 = SearchQuery.create_regex_query("^(.)\\1{1,}")  # Search for two or more identical characters at the beginning of a word
result2 = index.search(query2)

# Printing the words found in each document
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/regular-expression-search/documents/quantum-theory.txt), [documents/zoo-guide.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/regular-expression-search/documents/zoo-guide.txt), [documents/library-stats.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/regular-expression-search/documents/library-stats.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "regular-expression-search.txt" >}}  
```text
Documents found by the text query: 3
Documents found by the object query: 3
    library-stats.txt ['00', '550']
    quantum-theory.txt ['00']
    zoo-guide.txt ['00', 'aardvark', 'eel', 'llama']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/regular-expression-search/regular_expression_search/regular-expression-search.txt)
{{< /tab >}}
{{< /tabs >}}

In object form, a regular expression query can also be created with the [create_regex_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_regex_query/) method overload that takes a combination of [RegexOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search/regexoptions/) values as the second argument. The options must contain RegexOptions.IGNORE_CASE; otherwise, ValueError is raised. The overload with one argument uses RegexOptions.IGNORE_CASE. Python's `re` flags are accepted too and translated: `re.IGNORECASE` means RegexOptions.IGNORE_CASE.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, RegexOptions, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Creating a regular expression query with the specified options
regex_options = RegexOptions.IGNORE_CASE | RegexOptions.CULTURE_INVARIANT
query = SearchQuery.create_regex_query("^(.)\\1{1,}", regex_options)  # Search for two or more identical characters at the beginning of a word
result = index.search(query)

# Printing the words found in each document
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/regular-expression-search/documents/quantum-theory.txt), [documents/zoo-guide.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/regular-expression-search/documents/zoo-guide.txt), [documents/library-stats.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/regular-expression-search/documents/library-stats.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "regular-expression-search-2.txt" >}}  
```text
Documents found: 3
    library-stats.txt ['00', '550']
    quantum-theory.txt ['00']
    zoo-guide.txt ['00', 'aardvark', 'eel', 'llama']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/regular-expression-search/regular_expression_search_2/regular-expression-search-2.txt)
{{< /tab >}}
{{< /tabs >}}

Since version 26.10, regular expression search uses as many threads as there are processor cores by default. Therefore, the order of the found words in the search result may differ from run to run. To get the same order every time, set the [search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/search_threads/) index setting to NumberOfThreads.ONE.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
