---
id: faceted-search
url: search/python-net/faceted-search
title: Faceted search
weight: 5
description: "This article describes the creation of faceted search queries."
keywords: Faceted search, Faceted search definition
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page describes the creation of faceted search queries.

## Faceted search definition

Faceted search within GroupDocs.Search is a filtering of search results by setting valid document field names to search.

## Creating faceted search queries

Faceted search allows you to search only in certain fields of documents, for example, only in the content field or in the file name field. A simple faceted search example is presented below with queries in text and object form.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery
from groupdocs.search.options import CommonFieldNames

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search in the content field with text query
result1 = index.search("content: Einstein")

# Search in the content field with object query
word_query = SearchQuery.create_word_query("Einstein")
field_query = SearchQuery.create_field_query(CommonFieldNames.CONTENT, word_query)
result2 = index.search(field_query)

# Search in all fields, for comparison
result3 = index.search("Einstein")

# Printing the documents found and the fields in which the word was found
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path), [field.field_name for field in document.found_fields])
print("Documents found in all fields:", result3.document_count)
for document in result3:
    print("   ", os.path.basename(document.document_info.file_path), [field.field_name for field in document.found_fields])
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/relativity-primer.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/nobel-prize.txt), [documents/albert-einstein-letters.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/albert-einstein-letters.txt), [documents/supplier-list.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/supplier-list.txt), [documents/annual-report.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/annual-report.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "creating-faceted-search-queries.txt" >}}  
```text
Documents found by the text query: 2
Documents found by the object query: 2
    einstein-biography.txt ['content']
    nobel-prize.txt ['content']
Documents found in all fields: 3
    albert-einstein-letters.txt ['filename']
    einstein-biography.txt ['filename', 'content']
    nobel-prize.txt ['content']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/faceted-search/creating_faceted_search_queries/creating-faceted-search-queries.txt)
{{< /tab >}}
{{< /tabs >}}

Faceted search can be combined with other types of searches using parentheses. The following faceted search example demonstrates the same multi-faceted search query in text and object form. Both queries search for documents whose file name contains both the words "Albert" and "Einstein", or whose content contains the phrase "theory of relativity" or the phrase "special relativity".

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery
from groupdocs.search.options import CommonFieldNames

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search with text query
result1 = index.search("(filename: (Albert AND Einstein)) OR (content: (\"theory of relativity\" OR \"special relativity\"))")

# Search with object query
albert_query = SearchQuery.create_word_query("Albert")
einstein_query = SearchQuery.create_word_query("Einstein")
and_query = SearchQuery.create_and_query(albert_query, einstein_query)
filename_query = SearchQuery.create_field_query(CommonFieldNames.FILE_NAME, and_query)

theory_query = SearchQuery.create_word_query("theory")
of_query = SearchQuery.create_word_query("of")
relativity1_query = SearchQuery.create_word_query("relativity")
special_query = SearchQuery.create_word_query("special")
relativity2_query = SearchQuery.create_word_query("relativity")

phrase1_query = SearchQuery.create_phrase_search_query(theory_query, of_query, relativity1_query)
phrase2_query = SearchQuery.create_phrase_search_query(special_query, relativity2_query)
or_query = SearchQuery.create_or_query(phrase1_query, phrase2_query)
content_query = SearchQuery.create_field_query(CommonFieldNames.CONTENT, or_query)

root_query = SearchQuery.create_or_query(filename_query, content_query)
result2 = index.search(root_query)

# Printing the documents found and the fields in which they were found
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path), [field.field_name for field in document.found_fields])
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/relativity-primer.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/nobel-prize.txt), [documents/albert-einstein-letters.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/albert-einstein-letters.txt), [documents/supplier-list.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/supplier-list.txt), [documents/annual-report.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/annual-report.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "creating-faceted-search-queries-2.txt" >}}  
```text
Documents found by the text query: 3
Documents found by the object query: 3
    albert-einstein-letters.txt ['filename']
    einstein-biography.txt ['content']
    relativity-primer.txt ['content']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/faceted-search/creating_faceted_search_queries_2/creating-faceted-search-queries-2.txt)
{{< /tab >}}
{{< /tabs >}}

## Using format specific fields

For each document format, there are standard fields that may be present in documents of this type. The library provides the following classes containing constants with the names of standard document fields: [EpubFieldNames](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/epubfieldnames/), [FictionBookFieldNames](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/fictionbookfieldnames/), [MailFieldNames](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/mailfieldnames/), [PresentationFieldNames](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/presentationfieldnames/), [SpreadsheetFieldNames](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/spreadsheetfieldnames/), [WordsFieldNames](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/wordsfieldnames/).

There are also fields that may be present in documents of any type. The names of such fields are represented in the [CommonFieldNames](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/commonfieldnames/) class.

An example of using standard field names of documents is presented in the following example.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery
from groupdocs.search.options import WordsFieldNames

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search in the company field with text query
query1 = WordsFieldNames.COMPANY + ": Dycum"
result1 = index.search(query1)

# Search in the company field with object query
word_query = SearchQuery.create_word_query("Dycum")
field_query = SearchQuery.create_field_query(WordsFieldNames.COMPANY, word_query)
result2 = index.search(field_query)

# Printing the documents found and the fields in which the word was found
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path), [field.field_name for field in document.found_fields])
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/relativity-primer.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/nobel-prize.txt), [documents/albert-einstein-letters.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/albert-einstein-letters.txt), [documents/supplier-list.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/supplier-list.txt), [documents/annual-report.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/faceted-search/documents/annual-report.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "using-format-specific-fields.txt" >}}  
```text
Documents found by the text query: 1
Documents found by the object query: 1
    annual-report.docx ['company']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/faceted-search/using_format_specific_fields/using-format-specific-fields.txt)
{{< /tab >}}
{{< /tabs >}}

The following are the names of standard fields included in the library grouped by the formats containing them.

### Common fields

*   Content
    
*   FileName
    
*   FormatFamily
    
*   CreationDate
    
*   ModificationDate
    
*   EmbeddedImageOcr (the text recognized by OCR in images embedded in documents)
    

### Epub format fields

*   Title
    
*   Subject
    
*   Author
    
*   Description
    
*   Language
    
*   Copyrights
    
*   Publisher
    
*   PublishedDate
    

### Fiction Book format fields

*   Title
    
*   Subject
    
*   Keywords
    
*   Author
    
*   Description
    
*   Language
    
*   Publisher
    
*   PublishedDate
    

### Mail format fields

*   MailMessageBody
    
*   MailSenderName
    
*   MailDisplayTo
    
*   MailSubject
    

### Presentation format fields

*   Application
    
*   ApplicationVersion
    
*   Title
    
*   Subject
    
*   Comments
    
*   Keywords
    
*   ContentStatus
    
*   Category
    
*   Manager
    
*   Author
    
*   LastAuthor
    
*   Company
    
*   HyperlinkBase
    
*   CreatedTime
    
*   LastSavedTime
    
*   LastPrintedTime
    
*   RevisionNumber
    
*   TotalEditingTime
    

### Spreadsheet format fields

*   Application
    
*   ApplicationVersion
    
*   Title
    
*   Subject
    
*   Comments
    
*   Keywords
    
*   ContentStatus
    
*   Category
    
*   Manager
    
*   Author
    
*   LastAuthor
    
*   Company
    
*   HyperlinkBase
    
*   CreatedTime
    
*   LastSavedTime
    
*   LastPrintedTime
    

### Words format fields

*   Application
    
*   ApplicationVersion
    
*   Template
    
*   Title
    
*   Subject
    
*   Comments
    
*   Keywords
    
*   ContentStatus
    
*   Category
    
*   Manager
    
*   Author
    
*   LastAuthor
    
*   Company
    
*   HyperlinkBase
    
*   CreatedTime
    
*   LastSavedTime
    
*   LastPrintedTime
    
*   RevisionNumber
    
*   TotalEditingTime
    

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
