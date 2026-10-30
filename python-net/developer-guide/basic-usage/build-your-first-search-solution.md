---
id: build-your-first-search-solution
url: search/python-net/build-your-first-search-solution
title: Build your first search solution
weight: 1
description: "This article describes the basic steps of building a search solution with GroupDocs.Search for Python via .NET: creating and opening an index, indexing documents, searching and processing search results."
keywords: create an index, open an index, index documents, search, highlight search results, Python
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
    name: How to build a document search solution in Python
    description: Learn how to index documents and search them in Python step by step
    steps:
      - name: Create an index
        text: Create an object of the Index class. The constructor takes the path to the index folder; the index is created in the folder or loaded from it if it already exists there.
      - name: Subscribe to index events
        text: Add a handler that takes the sender and the event arguments to index.events.error_occurred to receive information about indexing errors.
      - name: Add documents to the index
        text: Call the add method with the path to a folder or a file. Set is_async of IndexingOptions to True to index asynchronously.
      - name: Search
        text: Call the search method with a search query. The method returns a SearchResult object.
      - name: Use the search results
        text: Get the found documents with get_found_document, or highlight the found words in the text of a document with the highlight method.
---
This brief guide describes the basic steps for creating your first search index.

## Step #1: Create new index

First of all you need to create an index. An index can be created in memory or on disk. An index created in memory is lost when your program exits unless it is saved to disk with the [save_to](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/save_to/) method. In contrast, an index created on disk may be loaded in the future to continue working. Details on creating an index are described in the section [Creating an index]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/_index.md" >}}). The following example shows how to create an index on disk.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an index in the specified folder
index = Index("./index")
print("Index folder:", index.index_info.index_folder)
```
{{< /tab >}}
{{< tab "step-1-create-new-index.txt" >}}  
```text
Index folder: index
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-your-first-search-solution/step_1_create_new_index/step-1-create-new-index.txt)
{{< /tab >}}
{{< /tabs >}}

## Step #2: Open existing index

To continue working with a previously created index, it must be loaded. The constructors of the **[Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/)** class that take an index folder path load the index if it exists in the specified folder, and create a new index otherwise. An existing index is overwritten only if the *overwrite_if_exists* parameter is *True*, for example `Index("./index", True)`. The constructors without a folder path, `Index()` and `Index(settings)`, create a new index in memory.

The **[Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/)** class is disposable. When an index is no longer needed, call its [dispose](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/dispose/) method or use the index in a *with* statement to release the resources used by the index. The following example creates an index, closes it, and then loads the existing index from the same folder.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an index and indexing documents; the with statement releases the index
with Index("./index") as index:
    index.add("./documents")

# Loading the existing index from the same folder
index = Index("./index")
print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/time.txt), [documents/protected.docx](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/protected.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "step-2-open-existing-index.txt" >}}  
```text
Indexed documents: 5
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-your-first-search-solution/step_2_open_existing_index/step-2-open-existing-index.txt)
{{< /tab >}}
{{< /tabs >}}

{{< alert style="info" >}}Since version 26.10, an index created by a previous version cannot be opened until it is updated with the [IndexUpdater](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/) class; the constructor raises [RuntimeError](https://docs.python.org/3/library/exceptions.html#RuntimeError). See [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}).{{< /alert >}}

## Step #3: Subscribe to index events

After creating an index, you need to add documents to the index for indexing. Indexing documents can be successful or unsuccessful for various reasons, for example, due to read errors from the disk or the presence of a password to access a document. To receive information about indexing errors, you can subscribe to the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event. An event handler is a function that takes the sender and the event arguments. To work with events, see the section [Search index events]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}). The following example shows how to subscribe to the error_occurred event; the documents folder contains a password-protected document whose password is not provided.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")

def on_error_occurred(sender, args):
    print(args.message)  # Writing error messages to the console

index.events.error_occurred += on_error_occurred

index.add("./documents")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/time.txt), [documents/protected.docx](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/protected.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "step-3-subscribe-to-index-events.txt" >}}  
```text
Error during text extraction from documents\protected.docx
GroupDocs.Parser.Exceptions.InvalidPasswordException: Exception of type 'GroupDocs.Parser.Exceptions.InvalidPasswordException' was thrown.
   at .(Stream , LoadOptions , Boolean )
   at .()
   at .()
   at GroupDocs.Parser.Parser.GetMetadata()
   at .()
   at .(Document ,  , String )
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-your-first-search-solution/step_3_subscribe_to_index_events/step-3-subscribe-to-index-events.txt)
{{< /tab >}}
{{< /tabs >}}

## Step #4: Add files synchronously

Document indexing can be performed synchronously or asynchronously. Synchronous indexing means that a thread that started the indexing process will be busy until the operation is completed. The following example shows how to perform indexing synchronously.

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")  # Creating an index in the specified folder

index.add("./documents")  # Synchronous indexing documents from the specified folder
print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/time.txt), [documents/protected.docx](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/protected.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "step-4-add-files-synchronously.txt" >}}  
```text
Indexed documents: 5
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-your-first-search-solution/step_4_add_files_synchronously/step-4-add-files-synchronously.txt)
{{< /tab >}}
{{< /tabs >}}

## Step #5: Add files asynchronously

More often, however, it is necessary to perform indexing asynchronously, with the ability to execute other tasks in the thread that launched the operation. A detailed description of all aspects of the indexing process is provided in the section [Indexing]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/_index.md" >}}). The following example shows how to perform indexing asynchronously. The event handler is called on an engine thread, so the example waits for the end of the operation with a *threading.Event*.

{{< tabs "example5" >}}
{{< tab "Python" >}}
```python
import threading

from groupdocs.search import Index
from groupdocs.search.options import IndexingOptions

# Creating an index
index = Index("./index")

finished = threading.Event()

# Subscribing to the event
def on_operation_finished(sender, args):
    # There should be a code indicating the completion of the operation
    print("Operation finished:", args.operation_type.name)
    finished.set()

index.events.operation_finished += on_operation_finished

# Setting the flag for asynchronous indexing
options = IndexingOptions()
options.is_async = True

# Asynchronous indexing documents from the specified folder
# The current method terminates before the operation completes
index.add("./documents", options)

# Other tasks can be performed here; then wait for the operation to complete
finished.wait()
print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/time.txt), [documents/protected.docx](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/protected.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "step-5-add-files-asynchronously.txt" >}}  
```text
Operation finished: INDEXING
Indexed documents: 5
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-your-first-search-solution/step_5_add_files_asynchronously/step-5-add-files-asynchronously.txt)
{{< /tab >}}
{{< /tabs >}}

## Step #6: Perform search

When documents are indexed, the index is ready to handle search queries. The following types of search queries are supported: simple, fuzzy, case sensitive, boolean, phrase, faceted, with wildcards, and others. Description of search queries of various types is presented in the section [Searching]({{< ref "search/python-net/developer-guide/advanced-usage/searching/_index.md" >}}). The example below indexes documents as in the previous examples and shows how to perform simple search in an index.

{{< tabs "example6" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = "Einstein"  # Specify a search query
result = index.search(query)  # Searching in the index
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/time.txt), [documents/protected.docx](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/protected.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "step-6-perform-search.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-your-first-search-solution/step_6_perform_search/step-6-perform-search.txt)
{{< /tab >}}
{{< /tabs >}}

## Step #7: Use search results

When a search is completed, you need to somehow interpret a result. The result can be represented by a simple list of documents found, or the words and phrases found can be highlighted in the text of the document. For more information on processing search results, see [Search results]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-results.md" >}}). The example below shows how to list found documents in the console.

{{< alert style="info" >}}In the evaluation mode, only the first 10 found documents can be retrieved: the [get_found_document](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/get_found_document/) method raises [LicenseRestrictionException](https://reference.groupdocs.com/search/python-net/groupdocs.search.exceptions/licenserestrictionexception/) for a document number of 10 or greater. See [Licensing and evaluation]({{< ref "search/python-net/getting-started/licensing-and-subscription.md" >}}).{{< /alert >}}

{{< tabs "example7" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")

query = "Einstein"  # Specify a search query

# Search in index
result = index.search(query)

# Printing the result
print("Documents found:", result.document_count)
print("Total occurrences found:", result.occurrence_count)
for i in range(result.document_count):
    document = result.get_found_document(i)
    print("\tDocument:", document.document_info.file_path)
    print("\tOccurrences:", document.occurrence_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/time.txt), [documents/protected.docx](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/protected.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "step-7-use-search-results.txt" >}}  
```text
Documents found: 2
Total occurrences found: 6
	Document: documents\einstein.txt
	Occurrences: 4
	Document: documents\nobel-prize-1921.txt
	Occurrences: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-your-first-search-solution/step_7_use_search_results/step-7-use-search-results.txt)
{{< /tab >}}
{{< /tabs >}}

The following example shows how to highlight search results in the text of a document. Detailed information on how to highlight search results is described in the section [Highlighting search results]({{< ref "search/python-net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}).

{{< tabs "example8" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import FileOutputAdapter
from groupdocs.search.highlighters import DocumentHighlighter
from groupdocs.search.options import OutputFormat

index = Index("./index")
index.add("./documents")

query = "Einstein"  # Specify a search query

result = index.search(query)  # Search in the index
if result.document_count > 0:
    document = result.get_found_document(0)  # Getting the first found document
    os.makedirs("./output", exist_ok=True)
    output_adapter = FileOutputAdapter(OutputFormat.HTML, "./output/Highlighted.html")  # Creating the output adapter to a file
    highlighter = DocumentHighlighter(output_adapter)  # Creating the highlighter object
    index.highlight(document, highlighter)  # Generating output HTML formatted document with highlighted search results
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/time.txt), [documents/protected.docx](/search/python-net/_sample_files/developer-guide/basic-usage/build-your-first-search-solution/documents/protected.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Highlighted.html" >}}  
```text
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>documents\einstein.txt</title>
</head>
<body>
<p>documents\einstein.txt</p>
<p></p>
<h3>filename</h3>
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/build-your-first-search-solution/step_7_use_search_results_2/Highlighted.html)
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
