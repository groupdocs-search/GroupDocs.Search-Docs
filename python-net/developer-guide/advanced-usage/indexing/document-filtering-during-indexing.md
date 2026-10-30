---
id: document-filtering-during-indexing
url: search/python-net/document-filtering-during-indexing
title: Document filtering during indexing
weight: 9
description: "This page contains a description of the use of document filters for indexing, as well as descriptions of all types of filters with examples of their creation."
keywords: document filters for indexing, document filters, DocumentFilter, skip documents, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains a description of the use of document filters for indexing, as well as descriptions of all types of filters with examples of their creation. Printing a filter shows its description.

## Setting a filter

To indicate which documents from added folders should be indexed and which should not, the [document_filter](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/document_filter/) property of the [IndexSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/) class can be used to specify a filter based on various properties of these documents. If a document added separately or located in an added folder does not match the filter, it is not indexed. The default value is `None`, which means that all added files will be indexed if their format is supported. The filter can be changed at any time, also for an existing index through the [Index.index_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/index_settings/) property; the next [update]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md" >}}) of the index applies the changed filter to the indexed folders. The following example demonstrates how to set a document filter for indexing.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, IndexSettings
from groupdocs.search.options import DocumentFilter

# Creating a filter that skips documents with extensions '.doc', '.docx', '.rtf'
settings = IndexSettings()
file_extension_filter = DocumentFilter.create_file_extension(".doc", ".docx", ".rtf")  # Creating file extension filter that allows only specified extensions
inverted_filter = DocumentFilter.create_not(file_extension_filter)  # Inverting file extension filter to allow all extensions except specified ones
settings.document_filter = inverted_filter

# Creating an index in the specified folder
index = Index("./index", settings)

# Indexing documents
index.add("./documents")

for document in index.get_indexed_documents():
    print("Indexed:", os.path.basename(document.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/documents/Einstein.txt), [documents/Summary.html](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/documents/Summary.html), [documents/Relativity.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/documents/Relativity.pdf), [documents/Lecture.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/documents/Lecture.docx), [documents/Notes.rtf](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/documents/Notes.rtf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "setting-a-filter.txt" >}}  
```text
Indexed: Einstein.txt
Indexed: Relativity.pdf
Indexed: Summary.html
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/setting_a_filter/setting-a-filter.txt)
{{< /tab >}}
{{< /tabs >}}

## Creation time filters

The first type of filters is based on the time the document file was created. Such filters can skip files created earlier than a certain date, later than a certain date, or outside a certain range of dates, given as `datetime.datetime` objects. Examples of these filters are given below.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import datetime

from groupdocs.search.options import DocumentFilter

# The first filter skips files created earlier than January 1, 2017, 00:00:00
filter1 = DocumentFilter.create_creation_time_lower_bound(datetime.datetime(2017, 1, 1))

# The second filter skips files created later than June 15, 2018, 00:00:00
filter2 = DocumentFilter.create_creation_time_upper_bound(datetime.datetime(2018, 6, 15))

# The third filter skips files created outside the range from January 1, 2017, 00:00:00 to June 15, 2018, 00:00:00
filter3 = DocumentFilter.create_creation_time_range(datetime.datetime(2017, 1, 1), datetime.datetime(2018, 6, 15))

print(filter1)
print(filter2)
print(filter3)
```
{{< /tab >}}
{{< tab "creation-time-filters.txt" >}}  
```text
(Creation time lower bound: 1/1/2017 12:00:00 AM)
(Creation time upper bound: 6/15/2018 12:00:00 AM)
(Creation time range: 1/1/2017 12:00:00 AM - 6/15/2018 12:00:00 AM)
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/creation_time_filters/creation-time-filters.txt)
{{< /tab >}}
{{< /tabs >}}

## Modification time filters

The next type of filters works similarly, but based on the document file modification date. Examples are presented below.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import datetime

from groupdocs.search.options import DocumentFilter

# The first filter skips files modified earlier than January 1, 2017, 00:00:00
filter1 = DocumentFilter.create_modification_time_lower_bound(datetime.datetime(2017, 1, 1))

# The second filter skips files modified later than June 15, 2018, 00:00:00
filter2 = DocumentFilter.create_modification_time_upper_bound(datetime.datetime(2018, 6, 15))

# The third filter skips files modified outside the range from January 1, 2017, 00:00:00 to June 15, 2018, 00:00:00
filter3 = DocumentFilter.create_modification_time_range(datetime.datetime(2017, 1, 1), datetime.datetime(2018, 6, 15))

print(filter1)
print(filter2)
print(filter3)
```
{{< /tab >}}
{{< tab "modification-time-filters.txt" >}}  
```text
(Modification time lower bound: 1/1/2017 12:00:00 AM)
(Modification time upper bound: 6/15/2018 12:00:00 AM)
(Modification time range: 1/1/2017 12:00:00 AM - 6/15/2018 12:00:00 AM)
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/modification_time_filters/modification-time-filters.txt)
{{< /tab >}}
{{< /tabs >}}

## File path filters

The next type of filters allows you to set a regular expression for skipping those documents whose full paths do not match the specified pattern. This type of filters uses the .NET **System.Text.RegularExpressions.Regex** class to compare with a pattern, so the pattern follows the .NET regular expression syntax. The pattern can be specified with or without [RegexOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search/regexoptions/).

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import RegexOptions
from groupdocs.search.options import DocumentFilter

# The filter skips files that do not contain the word 'Einstein' in their paths, not case sensitive
document_filter = DocumentFilter.create_file_path_regular_expression("Einstein", RegexOptions.IGNORE_CASE)

print(document_filter)
```
{{< /tab >}}
{{< tab "file-path-filters.txt" >}}  
```text
(File name regex: Einstein)
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/file_path_filters/file-path-filters.txt)
{{< /tab >}}
{{< /tabs >}}

## File length filters

The following group of filters uses the file length in bytes for filtering. It is possible to specify a lower bound, an upper bound or the range of acceptable file length. Examples are below.

{{< tabs "example5" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.options import DocumentFilter

# The first filter skips documents less than 50 KB in length
filter1 = DocumentFilter.create_file_length_lower_bound(50 * 1024)

# The second filter skips documents more than 10 MB in length
filter2 = DocumentFilter.create_file_length_upper_bound(10 * 1024 * 1024)

# The third filter skips documents less than 100 KB and more than 5 MB in length
filter3 = DocumentFilter.create_file_length_range(100 * 1024, 5 * 1024 * 1024)

print(filter1)
print(filter2)
print(filter3)
```
{{< /tab >}}
{{< tab "file-length-filters.txt" >}}  
```text
(File length lower bound: 51200)
(File length upper bound: 10485760)
(File length range: 102400 - 5242880)
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/file_length_filters/file-length-filters.txt)
{{< /tab >}}
{{< /tabs >}}

## File extension filter

The following type of filters allows you to specify a list of valid file extensions for indexing.

{{< tabs "example6" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.options import DocumentFilter

# This filter allows indexing only FB2, EPUB, and TXT files
document_filter = DocumentFilter.create_file_extension(".fb2", ".epub", ".txt")

print(document_filter)
```
{{< /tab >}}
{{< tab "file-extension-filter.txt" >}}  
```text
(File extensions: .fb2, .epub, .txt)
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/file_extension_filter/file-extension-filter.txt)
{{< /tab >}}
{{< /tabs >}}

## Logical NOT filter

The next type of filter allows you to invert the logic of an internal filter.

{{< tabs "example7" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import IndexSettings
from groupdocs.search.options import DocumentFilter

settings = IndexSettings()
document_filter = DocumentFilter.create_file_extension(".htm", ".html")
inverted_filter = DocumentFilter.create_not(document_filter)  # Inverting file extension filter to allow all extensions except of HTM and HTML
settings.document_filter = inverted_filter

print(settings.document_filter)
```
{{< /tab >}}
{{< tab "logical-not-filter.txt" >}}  
```text
(NOT (File extensions: .htm, .html))
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/logical_not_filter/logical-not-filter.txt)
{{< /tab >}}
{{< /tabs >}}

## Logical AND filter

The filter of the following type allows you to compose a complex filter from several other filters, using the logic "AND". This composite filter requires the simultaneous fulfillment of the conditions of all internal filters for each file to be added. The example below shows how to make a filter that allows indexing only documents created in the date range from 01/01/2015 to 01/01/2016, with the extension '.txt' and a size of no more than 8 MB.

{{< tabs "example8" >}}
{{< tab "Python" >}}
```python
import datetime

from groupdocs.search import IndexSettings
from groupdocs.search.options import DocumentFilter

settings = IndexSettings()
filter1 = DocumentFilter.create_creation_time_range(datetime.datetime(2015, 1, 1), datetime.datetime(2016, 1, 1))
filter2 = DocumentFilter.create_file_extension(".txt")
filter3 = DocumentFilter.create_file_length_upper_bound(8 * 1024 * 1024)
final_filter = DocumentFilter.create_and(filter1, filter2, filter3)
settings.document_filter = final_filter

print(settings.document_filter)
```
{{< /tab >}}
{{< tab "logical-and-filter.txt" >}}  
```text
((Creation time range: 1/1/2015 12:00:00 AM - 1/1/2016 12:00:00 AM) AND (File extensions: .txt) AND (File length upper bound: 8388608))
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/logical_and_filter/logical-and-filter.txt)
{{< /tab >}}
{{< /tabs >}}

## Logical OR filter

The filter of the following type allows you to compose a complex filter from several other filters, using the logic "OR". This composite filter requires fulfillment of the condition of at least one internal filter for each added file. The example below shows how to create a filter that limits the size of text files to 5 MB and other files to 10 MB.

{{< tabs "example9" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import IndexSettings
from groupdocs.search.options import DocumentFilter

settings = IndexSettings()
txt_filter = DocumentFilter.create_file_extension(".txt")
not_txt_filter = DocumentFilter.create_not(txt_filter)
bound5_filter = DocumentFilter.create_file_length_upper_bound(5 * 1024 * 1024)
bound10_filter = DocumentFilter.create_file_length_upper_bound(10 * 1024 * 1024)
txt_size_filter = DocumentFilter.create_and(txt_filter, bound5_filter)
not_txt_size_filter = DocumentFilter.create_and(not_txt_filter, bound10_filter)
final_filter = DocumentFilter.create_or(txt_size_filter, not_txt_size_filter)
settings.document_filter = final_filter

print(settings.document_filter)
```
{{< /tab >}}
{{< tab "logical-or-filter.txt" >}}  
```text
(((File extensions: .txt) AND (File length upper bound: 5242880)) OR ((NOT (File extensions: .txt)) AND (File length upper bound: 10485760)))
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-filtering-during-indexing/logical_or_filter/logical-or-filter.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
