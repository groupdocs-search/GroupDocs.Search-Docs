---
id: search-index-events
url: search/python-net/search-index-events
title: Search index events
weight: 1
description: "This page contains information about the purpose and use of all index events in GroupDocs.Search for Python via .NET."
keywords: index events, events, subscribing to event, track progress
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains information about the purpose and use of all index events.

## Subscribing to events

The events of an index are available through the [events](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/events/) property of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class, which returns an [EventHub](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/) object. A handler is a function, a lambda or another callable that takes two arguments: the sender and the event arguments. Subscribe a handler with `+=`, for example `index.events.file_indexing += on_file_indexing`; `-=` removes the subscription made last with the handler, and `clear()` removes all the handlers of the event, for example `index.events.file_indexing.clear()`.

A handler runs on the thread that raises the event:

*   During a synchronous operation with one indexing thread (the default), the events are raised on the thread that called the method, for example, the method `add`.
*   If [IndexingOptions.threads](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/threads/) is greater than 1, the events raised for each document ([file_indexing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/file_indexing/), [password_required](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/password_required/), [operation_progress_changed](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/operation_progress_changed/) and [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/)) are raised on engine worker threads, several at the same time.
*   During an asynchronous operation ([IndexingOptions.is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/is_async/) is `True`), the method returns before the operation completes, and the events that follow are raised on engine worker threads. Wait for the [operation_finished](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/operation_finished/) event, for example with a `threading.Event`, before the script uses the results or ends.

An exception that a handler raises on the calling thread reaches the engine: for example, an exception raised by a `file_indexing` handler is reported by the `error_occurred` event, and the document is not indexed. An exception raised on an engine worker thread is printed to stderr and ignored.

## operation_finished event

The [operation_finished](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/operation_finished/) event occurs when an index operation completes. The [operation_type](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/operationfinishedeventargs/operation_type/) property of the event arguments returns a value of the [OperationType](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/operationtype/) enumeration:

*   **INDEXING** - adding documents to an index;
*   **UPDATING** - updating documents in an index;
*   **MERGING** - merging indexes;
*   **OPTIMIZING** - optimizing an index (merging its segments);
*   **DELETING** - deleting documents from an index by paths or by document keys.

This event can be used to receive notification of the completion of an asynchronous operation. In the following example, the `add` method returns at once, and the event is raised on an engine worker thread; the script waits for it with a `threading.Event`.

{{< tabs "example1" >}}
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
    # Writing operation details to the console
    print("Operation finished:", args.operation_type.name)
    print("Message:", args.message)
    print("Index folder:", args.index_folder)
    print("Time:", args.time)
    finished.set()


index.events.operation_finished += on_operation_finished

# Indexing documents from the specified folder
options = IndexingOptions()
options.is_async = True  # Enabling asynchronous indexing mode
index.add("./documents", options)

# The method returns before the operation completes, so waiting for the event
finished.wait()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/curie.txt), [documents/important-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/important-notes.txt), [documents/protected.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/protected.pdf), [documents/report.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/report.pdf), [documents/photos.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/photos.docx), [documents/laboratory.jpg](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/laboratory.jpg), [documents/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "operation-finished-event.txt" >}}  
```text
Operation finished: INDEXING
Message: Files has been added to Index e180c83d-2c46-4cff-b4d6-8be087de3682.
Index folder: index
Time: 2026-10-29 19:04:58.548210+02:00
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-events/operation_finished_event/operation-finished-event.txt)
{{< /tab >}}
{{< /tabs >}}

## error_occurred event

The [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event occurs when an error happens in an index. Errors in an index can be caused, for example, by file system errors or unsupported formats of indexed documents. The [is_critical](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/indexerroreventargs/is_critical/) property of the event arguments indicates a critical error after which the index gets the [FAILED](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexstatus/) status and has to be opened again. The [check_segments](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/check_segments/) method also reports the damage found and the repairs made through this event (see [Checking index integrity]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}})). An example of receiving error notifications in the index is presented below.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an index
index = Index("./index")


# Subscribing to the event
def on_error_occurred(sender, args):
    # Writing an error message to the console
    print(("Critical error: " if args.is_critical else "Error: ") + args.message)


index.events.error_occurred += on_error_occurred

# Indexing documents from the specified folder
index.add("./documents")

# Searching in the index
result = index.search("Einstein")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/curie.txt), [documents/important-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/important-notes.txt), [documents/protected.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/protected.pdf), [documents/report.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/report.pdf), [documents/photos.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/photos.docx), [documents/laboratory.jpg](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/laboratory.jpg), [documents/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "error-occurred-event.txt" >}}  
```text
Error: Error during text extraction from documents\damaged.pdf
GroupDocs.Parser.Exceptions.UnsupportedDocumentFormatException: Exception of type 'GroupDocs.Parser.Exceptions.UnsupportedDocumentFormatException' was thrown.
   at .(Stream , LoadOptions )
   at .()
   at GroupDocs.Parser.Parser.GetMetadata()
   at .()
   at .(Document ,  , String )
Error: Error during text extraction from documents\protected.pdf
GroupDocs.Parser.Exceptions.InvalidPasswordExc
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-events/error_occurred_event/error-occurred-event.txt)
{{< /tab >}}
{{< /tabs >}}

## operation_progress_changed event

The [operation_progress_changed](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/operation_progress_changed/) event occurs when the progress of an indexing or updating operation changes. The event arguments ([OperationProgressEventArgs](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/operationprogresseventargs/)) contain the total number of documents to process, the numbers of processed and skipped documents, the progress percentage, as well as the key, the path and the status of the last processed document. The example below demonstrates how to track the progress of an index operation.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an index
index = Index("./index")


# Subscribing to the event
def on_operation_progress_changed(sender, args):
    print("Last processed:", args.last_document_path)
    print("Result:", args.last_document_status.name)
    print("Processed documents:", args.processed_documents)
    print("Skipped documents:", args.skipped_documents)
    print("Total documents:", args.total_documents)
    print("Progress percentage:", args.progress_percentage)


index.events.operation_progress_changed += on_operation_progress_changed

# Indexing documents from the specified folder
index.add("./documents")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/curie.txt), [documents/important-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/important-notes.txt), [documents/protected.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/protected.pdf), [documents/report.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/report.pdf), [documents/photos.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/photos.docx), [documents/laboratory.jpg](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/laboratory.jpg), [documents/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "operation-progress-changed-event.txt" >}}  
```text
Last processed: documents\curie.txt
Result: SUCCESSFULLY_PROCESSED
Processed documents: 1
Skipped documents: 0
Total documents: 9
Progress percentage: 11.11111111111111
Last processed: documents\damaged.pdf
Result: PROCESSED_WITH_ERROR
Processed documents: 2
Skipped documents: 0
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-events/operation_progress_changed_event/operation-progress-changed-event.txt)
{{< /tab >}}
{{< /tabs >}}

## optimization_progress_changed event

The [optimization_progress_changed](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/optimization_progress_changed/) event occurs when the progress of an index optimization operation changes. The example below demonstrates how to track the progress of the index optimization operation. Each call of the `add` method creates at least one new segment in the index, so the example adds the documents one by one before optimizing the index.

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an index and adding documents in separate operations, which creates several segments
index = Index("./index")
index.add("./documents/einstein-notes.txt")
index.add("./documents/newton.txt")
index.add("./documents/curie.txt")


# Subscribing to the event
def on_optimization_progress_changed(sender, args):
    print("Processed segments:", args.processed_segments)
    print("Total segments:", args.total_segments)
    print("Progress percentage:", args.progress_percentage)


index.events.optimization_progress_changed += on_optimization_progress_changed

# Optimizing the index
index.optimize()
```
{{< /tab >}}
{{< tab "documents/einstein-notes.txt" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/einstein-notes.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "documents/newton.txt" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "documents/curie.txt" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "optimization-progress-changed-event.txt" >}}  
```text
Processed segments: 2
Total segments: 3
Progress percentage: 66.66666666666667
Processed segments: 3
Total segments: 3
Progress percentage: 100
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-events/optimization_progress_changed_event/optimization-progress-changed-event.txt)
{{< /tab >}}
{{< /tabs >}}

## password_required event

The [password_required](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/password_required/) event occurs when an index requires a password to open a document. The event is raised before a document is opened, also for a document that is not protected, so the handler sets a password only for the documents that need it. An example of processing this event is presented below.

{{< tabs "example5" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an index
index = Index("./index")


# Subscribing to the event
def on_password_required(sender, args):
    if args.document_full_path.lower().endswith("protected.pdf"):
        args.password = "123456"
        print("Password supplied for:", args.document_full_path)


index.events.password_required += on_password_required

# Indexing documents from the specified folder
index.add("./documents")

# Searching for a word of the protected document
result = index.search("confidential")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/curie.txt), [documents/important-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/important-notes.txt), [documents/protected.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/protected.pdf), [documents/report.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/report.pdf), [documents/photos.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/photos.docx), [documents/laboratory.jpg](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/laboratory.jpg), [documents/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "password-required-event.txt" >}}  
```text
Password supplied for: documents\protected.pdf
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-events/password_required_event/password-required-event.txt)
{{< /tab >}}
{{< /tabs >}}

## file_indexing event

The [file_indexing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/file_indexing/) event occurs immediately before the start of indexing a document. This event can be used for

*   Skipping indexing of the current document (see also [Document filtering during indexing]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}}) page);
*   Specifying the encoding of the current text document (see also [Text file encoding detection]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/text-file-encoding-detection.md" >}}) page);
*   Specifying a custom text extractor for the current document (see also [Custom text extractors]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}) page);
*   Setting additional arbitrary fields for the current document;
*   Setting the [attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/fileindexingeventargs/attributes/) of the current document (see also [Document attributes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) page);
*   Specifying a custom [word splitter](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/fileindexingeventargs/word_splitter/) for the current document (see also [Custom text segmenter]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/custom-text-segmenter.md" >}}) page).

The following example demonstrates how to add an additional field to documents whose names end in "report.pdf" and how to skip indexing documents containing "important" text in their paths. The search at the end finds the documents by the added field.

{{< tabs "example6" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.common import DocumentField

# Creating an index
index = Index("./index")


# Subscribing to the event
def on_file_indexing(sender, args):
    path = args.document_full_path.lower()
    if path.endswith("report.pdf"):
        args.additional_fields = [DocumentField("Tags", "Report")]
        print("Tagged:", args.document_full_path)
    if "important" in path:
        args.skip_indexing = True
        print("Skipped:", args.document_full_path)


index.events.file_indexing += on_file_indexing

# Indexing documents from the specified folder
index.add("./documents")

# Searching in the added field
result = index.search("Tags:Report")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/curie.txt), [documents/important-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/important-notes.txt), [documents/protected.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/protected.pdf), [documents/report.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/report.pdf), [documents/photos.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/photos.docx), [documents/laboratory.jpg](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/laboratory.jpg), [documents/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "file-indexing-event.txt" >}}  
```text
Skipped: documents\important-notes.txt
Tagged: documents\report.pdf
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-events/file_indexing_event/file-indexing-event.txt)
{{< /tab >}}
{{< /tabs >}}

## status_changed event

The [status_changed](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/status_changed/) event occurs when an index status changes. The new status is available in the [status](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/baseindexeventargs/status/) property of the event arguments. The following example demonstrates how to use this event to notify the completion of an index operation. The operation is asynchronous, so the handler receives the final status on an engine worker thread, and the script waits for the notification.

{{< tabs "example7" >}}
{{< tab "Python" >}}
```python
import threading

from groupdocs.search import Index
from groupdocs.search.common import IndexStatus
from groupdocs.search.options import IndexingOptions

# Creating an index
index = Index("./index")

completed = threading.Event()


# Subscribing to the event
def on_status_changed(sender, args):
    if args.status == IndexStatus.READY or args.status == IndexStatus.FAILED:
        # A notification of the operation completion
        print("The operation is completed. Index status:", args.status.name)
        completed.set()


index.events.status_changed += on_status_changed

# Setting the flag for asynchronous indexing
options = IndexingOptions()
options.is_async = True

# Asynchronous indexing documents from the specified folder
# The method terminates before the operation completes
index.add("./documents", options)

# Waiting for the notification
completed.wait()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/curie.txt), [documents/important-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/important-notes.txt), [documents/protected.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/protected.pdf), [documents/report.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/report.pdf), [documents/photos.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/photos.docx), [documents/laboratory.jpg](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/laboratory.jpg), [documents/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "status-changed-event.txt" >}}  
```text
The operation is completed. Index status: READY
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-events/status_changed_event/status-changed-event.txt)
{{< /tab >}}
{{< /tabs >}}

## search_phase_completed event

The [search_phase_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/search_phase_completed/) event occurs when a phase (or stage) of a search operation in an index completes. This event is used to study intermediate search results when tuning search queries. Information on the phases of different types of search is presented on the page [Search flow]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-flow.md" >}}). The following example demonstrates the use of this event.

{{< tabs "example8" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import SearchOptions

# Creating an index
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")


# Subscribing to the event
def on_search_phase_completed(sender, args):
    words = args.words
    print("Search phase:", args.search_phase.name)
    print("Words:", len(words))
    for word in words:
        print("\t" + word)
    print()


index.events.search_phase_completed += on_search_phase_completed

options = SearchOptions()
options.use_synonym_search = True
options.use_word_forms_search = True
options.fuzzy_search.enabled = True
options.use_homophone_search = True
result = index.search("Einstein", options)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/curie.txt), [documents/important-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/important-notes.txt), [documents/protected.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/protected.pdf), [documents/report.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/report.pdf), [documents/photos.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/photos.docx), [documents/laboratory.jpg](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/laboratory.jpg), [documents/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-phase-completed-event.txt" >}}  
```text
Search phase: ALIAS_SUBSTITUTION
Words: 1
	Einstein

Search phase: HOMOPHONE_SEARCH
Words: 1
	einstein

Search phase: SYNONYM_SEARCH
Words: 1
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-events/search_phase_completed_event/search-phase-completed-event.txt)
{{< /tab >}}
{{< /tabs >}}

## image_preparing event

The [image_preparing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/image_preparing/) event occurs immediately before adding indexed image to an index. The event can be used, for example, to save an image separately from its containing document, since it provides an image data stream ([image_stream](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/imagepreparingeventargs/image_stream/)).

The event is raised for every separate image file, whether or not image indexing is enabled. When [enabled_for_separate_images](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imageindexingoptions/enabled_for_separate_images/) is `False`, the image is not indexed and [image_frames](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/imagepreparingeventargs/image_frames/) is empty. For an image embedded in a document, the event is raised only when [enabled_for_embedded_images](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imageindexingoptions/enabled_for_embedded_images/) is `True`.

The following example demonstrates the use of this event. It enables the image indexing options, which index images for [reverse image search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/reverse-image-search.md" >}}).

{{< tabs "example9" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import IndexingOptions

# Creating an index
index = Index("./index")


# Subscribing to the event
def on_image_preparing(sender, args):
    print("Document:", args.document_key)
    print("Image inner path:", "/".join(args.inner_path))
    print("Image index:", args.image_index)
    print("Image frames:", len(args.image_frames))


index.events.image_preparing += on_image_preparing

# Indexing images of separate image files and images embedded in documents
options = IndexingOptions()
options.image_indexing_options.enabled_for_separate_images = True
options.image_indexing_options.enabled_for_embedded_images = True

# Indexing files
index.add("./documents", options)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/curie.txt), [documents/important-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/important-notes.txt), [documents/protected.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/protected.pdf), [documents/report.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/report.pdf), [documents/photos.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/photos.docx), [documents/laboratory.jpg](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/laboratory.jpg), [documents/damaged.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/search-index-events/documents/damaged.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "image-preparing-event.txt" >}}  
```text
Document: documents\laboratory.jpg
Image inner path: 
Image index: 0
Image frames: 1
Document: documents\photos.docx
Image inner path: 
Image index: 0
Image frames: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/search-index-events/image_preparing_event/image-preparing-event.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
