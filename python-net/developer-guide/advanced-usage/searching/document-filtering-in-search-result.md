---
id: document-filtering-in-search-result
url: search/python-net/document-filtering-in-search-result
title: Document filtering in search result
weight: 4
description: "This article describes the document filters used to filter search results."
keywords: Document filtering, Document filtering in search result
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains a description of the document filters used during the search.

## Setting a filter

To specify which of the documents found should be returned as a result of the search, the [search_document_filter](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/search_document_filter/) property of the [SearchOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/) class is used. If the document found does not match the filter, it will not be returned. The default value is None, which means that all documents found will be returned. The following example shows how to set a document filter for searching.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchDocumentFilter, SearchOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Creating a search options object
options = SearchOptions()
options.search_document_filter = SearchDocumentFilter.create_file_extension(".txt")  # Setting a document filter

# Search in the index
# Only text documents will be returned as the result of the search
result = index.search("relativity", options)

# Printing the documents found
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-biography.txt), [documents/relativity-primer.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-primer.docx), [documents/relativity-guide.epub](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-guide.epub), [documents/relativity-lecture.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-lecture.pdf), [documents/einstein-relativity.fb2](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-relativity.fb2)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "setting-a-filter.txt" >}}  
```text
Documents found: 1
    einstein-biography.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/setting_a_filter/setting-a-filter.txt)
{{< /tab >}}
{{< /tabs >}}

## File path filters

The first supported type of search document filters allows you to set a regular expression for getting those documents whose full paths match the specified pattern. This type of filters is created with the [create_file_path_regular_expression](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchdocumentfilter/create_file_path_regular_expression/) method and uses the .NET **System.Text.RegularExpressions.Regex** class to compare with a pattern, so the pattern is written in .NET regular expression syntax. The method has two overloads: with a pattern only and with a pattern and [RegexOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search/regexoptions/).

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, RegexOptions
from groupdocs.search.options import SearchDocumentFilter, SearchOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# The filter returns only files that contain the word 'Einstein' in their paths, not case sensitive
document_filter = SearchDocumentFilter.create_file_path_regular_expression("Einstein", RegexOptions.IGNORE_CASE)

# Search in the index with the filter
options = SearchOptions()
options.search_document_filter = document_filter
result = index.search("relativity", options)

# Printing the documents found
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-biography.txt), [documents/relativity-primer.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-primer.docx), [documents/relativity-guide.epub](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-guide.epub), [documents/relativity-lecture.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-lecture.pdf), [documents/einstein-relativity.fb2](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-relativity.fb2)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "file-path-filters.txt" >}}  
```text
Documents found: 2
    einstein-biography.txt
    einstein-relativity.fb2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/file_path_filters/file-path-filters.txt)
{{< /tab >}}
{{< /tabs >}}

## File extension filter

The next supported type of search document filters allows you to specify a list of file extensions of the documents to be returned.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchDocumentFilter, SearchOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# This filter returns only FB2 and EPUB documents
document_filter = SearchDocumentFilter.create_file_extension(".fb2", ".epub")

# Search in the index with the filter
options = SearchOptions()
options.search_document_filter = document_filter
result = index.search("relativity", options)

# Printing the documents found
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-biography.txt), [documents/relativity-primer.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-primer.docx), [documents/relativity-guide.epub](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-guide.epub), [documents/relativity-lecture.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-lecture.pdf), [documents/einstein-relativity.fb2](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-relativity.fb2)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "file-extension-filter.txt" >}}  
```text
Documents found: 2
    einstein-relativity.fb2
    relativity-guide.epub
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/file_extension_filter/file-extension-filter.txt)
{{< /tab >}}
{{< /tabs >}}

## Attribute filter

The next supported type of search document filters allows you to return only those documents with which at least one of the specified text attributes is associated. You can learn more about attributes on the [Document attributes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) page.

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import AttributeChangeBatch
from groupdocs.search.options import SearchDocumentFilter, SearchOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Associating the attribute "main" with one of the indexed documents
batch = AttributeChangeBatch()
batch.add(os.path.abspath("./documents/relativity-lecture.pdf"), ["main"])
index.change_attributes(batch)

# This filter returns only documents that have attribute "main"
document_filter = SearchDocumentFilter.create_attribute("main")

# Search in the index with the filter
options = SearchOptions()
options.search_document_filter = document_filter
result = index.search("relativity", options)

# Printing the documents found
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-biography.txt), [documents/relativity-primer.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-primer.docx), [documents/relativity-guide.epub](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-guide.epub), [documents/relativity-lecture.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-lecture.pdf), [documents/einstein-relativity.fb2](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-relativity.fb2)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "attribute-filter.txt" >}}  
```text
Documents found: 1
    relativity-lecture.pdf
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/attribute_filter/attribute-filter.txt)
{{< /tab >}}
{{< /tabs >}}

## Combining filters

Search document filters can be combined using composite filters AND, OR, NOT. The [create_and](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchdocumentfilter/create_and/) and [create_or](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchdocumentfilter/create_or/) methods take two or more filters. The following example shows how to combine search document filters.

{{< tabs "example5" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, RegexOptions
from groupdocs.search.options import SearchDocumentFilter, SearchOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Creating an AND composite filter that returns all FB2 and EPUB documents that have the word 'Einstein' in their full paths
filter1 = SearchDocumentFilter.create_file_path_regular_expression("Einstein", RegexOptions.IGNORE_CASE)
filter2 = SearchDocumentFilter.create_file_extension(".fb2", ".epub")
and_filter = SearchDocumentFilter.create_and(filter1, filter2)

# Creating an OR composite filter that returns all DOC, DOCX, PDF and all documents that have the word Einstein in their full paths
filter3 = SearchDocumentFilter.create_file_path_regular_expression("Einstein", RegexOptions.IGNORE_CASE)
filter4 = SearchDocumentFilter.create_file_extension(".doc", ".docx", ".pdf")
or_filter = SearchDocumentFilter.create_or(filter3, filter4)

# Creating a filter that returns all found documents except TXT documents
filter5 = SearchDocumentFilter.create_file_extension(".txt")
not_filter = SearchDocumentFilter.create_not(filter5)

# Searching with each of the composite filters
for document_filter in (and_filter, or_filter, not_filter):
    options = SearchOptions()
    options.search_document_filter = document_filter
    result = index.search("relativity", options)
    print(document_filter, "- documents found:", result.document_count)
    for document in result:
        print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-biography.txt), [documents/relativity-primer.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-primer.docx), [documents/relativity-guide.epub](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-guide.epub), [documents/relativity-lecture.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/relativity-lecture.pdf), [documents/einstein-relativity.fb2](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/documents/einstein-relativity.fb2)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "combining-filters.txt" >}}  
```text
((File name regex: Einstein) AND (File extensions: .fb2, .epub)) - documents found: 1
    einstein-relativity.fb2
((File name regex: Einstein) OR (File extensions: .doc, .docx, .pdf)) - documents found: 4
    einstein-biography.txt
    einstein-relativity.fb2
    relativity-lecture.pdf
    relativity-primer.docx
(NOT (File extensions: .txt)) - documents found: 4
    einstein-relativity.fb2
    relativity-guide.epub
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/document-filtering-in-search-result/combining_filters/combining-filters.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
