---
id: getting-indexed-documents
url: search/python-net/getting-indexed-documents
title: Getting indexed documents
weight: 7
description: "This article explains how to get a list of indexed documents from an index, and how to get the text of indexed documents in HTML or plain text format."
keywords: Getting indexed documents
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains a description of how to get a list of indexed documents from an index, and how to get the text of indexed documents in HTML or plain text format.

## Getting indexed documents

To get a list of indexed documents from an index, use the [get_indexed_documents](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_indexed_documents/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class. Documents with the extensions ZIP, PST, OST can also contain internal documents. To get a list of internal documents, use the [get_indexed_document_items](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_indexed_document_items/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class. For ZIP archives, this way you can access documents of arbitrary nesting depth. An example of obtaining a list of documents from an index is presented below.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Getting list of indexed documents
documents = index.get_indexed_documents()
for document in documents:
    print(document.file_path)
    items = index.get_indexed_document_items(document)  # Getting list of document items
    for item in items:
        print("\t" + item.inner_path)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/getting-indexed-documents/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/getting-indexed-documents/documents/relativity-primer.txt), [documents/letters-archive.zip](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/getting-indexed-documents/documents/letters-archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "getting-indexed-documents.txt" >}}  
```text
documents\einstein-biography.txt
documents\letters-archive.zip
	letters\letter-1905.txt
	letters\letter-1915.txt
	readme.txt
documents\relativity-primer.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/getting-indexed-documents/getting_indexed_documents/getting-indexed-documents.txt)
{{< /tab >}}
{{< /tabs >}}

To get a single indexed document, use the [get_indexed_document](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_indexed_document/) method. It takes the file path of a document indexed from a file (as returned by the [DocumentInfo.file_path](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/file_path/) property, which is a full path) or the key of a document indexed from a stream or a structure, and returns None if there is no such document in the index.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Getting an indexed document by its file path
document = index.get_indexed_document(os.path.abspath("./documents/einstein-biography.txt"))
if document is not None:
    print(document.file_path + " (" + str(document.file_type) + ")")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/getting-indexed-documents/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/getting-indexed-documents/documents/relativity-primer.txt), [documents/letters-archive.zip](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/getting-indexed-documents/documents/letters-archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "getting-indexed-documents-2.txt" >}}  
```text
documents\einstein-biography.txt (.txt)
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/getting-indexed-documents/getting_indexed_documents_2/getting-indexed-documents-2.txt)
{{< /tab >}}
{{< /tabs >}}

## Getting text of indexed documents

The text of an indexed document can be extracted from an index if the option to save the text of documents in the index was enabled. If this option was not enabled when creating an index, then when the [get_document_text](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_document_text/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class is called, the text of the document will be extracted from the document again. Details about saving the text of documents in an index can be found on the page [Storing text of indexed documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}).

The generated text of the document is passed to an instance of a class derived from the abstract class [OutputAdapter](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/outputadapter/). Details on the output adapters are presented on the page [Output adapters]({{< ref "search/python-net/developer-guide/advanced-usage/searching/output-adapters.md" >}}).

After generating the text of a document into a file, this file can be opened by an Internet browser. The following example shows how to extract document text from an index.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import FileOutputAdapter
from groupdocs.search.options import OutputFormat

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Getting list of indexed documents
documents = index.get_indexed_documents()

# Getting a document text
if len(documents) > 0:
    os.makedirs("./output", exist_ok=True)
    output_adapter = FileOutputAdapter(OutputFormat.HTML, "./output/Text.html")
    index.get_document_text(documents[0], output_adapter)
    print("The text of", os.path.basename(documents[0].file_path), "is saved to ./output/Text.html")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/getting-indexed-documents/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/getting-indexed-documents/documents/relativity-primer.txt), [documents/letters-archive.zip](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/getting-indexed-documents/documents/letters-archive.zip)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Text.html" >}}  
```text
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>documents\einstein-biography.txt</title>
</head>
<body>
<p>documents\einstein-biography.txt</p>
<p></p>
<h3>filename</h3>
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/getting-indexed-documents/getting_text_of_indexed_documents/Text.html)
{{< /tab >}}
{{< /tabs >}}

To extract the text of a document from an index, there is also an overload of the method that takes an instance of the [TextOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textoptions/) class as a parameter. In this class, the following options can be specified:

*   [custom_extractor](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textoptions/custom_extractor/) is a custom extractor used during indexing, it is necessary if the text of the document was not saved in the index;
*   [additional_fields](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textoptions/additional_fields/) are additional document fields added during document indexing which are also necessary if the document text was not saved in the index;
*   [cancellation](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textoptions/cancellation/) is an object used to cancel the operation;
*   [generate_head](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textoptions/generate_head/) is a flag to specify whether the Head tag is generated in the output HTML; the default value is True;
*   [metadata_indexing_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/metadata_indexing_options/) is an object for specifying metadata indexing options. This property and the other properties inherited from the [BaseOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/) class (for example, [encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/encoding/), [auto_detect_encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/auto_detect_encoding/) and [ocr_indexing_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/ocr_indexing_options/)) specify how the text of the document is extracted; they are necessary if the document text was not saved in the index.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
