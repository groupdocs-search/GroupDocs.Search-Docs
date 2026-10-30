---
id: indexing-options
url: search/python-net/indexing-options
title: Indexing options
weight: 15
description: "This page contains a description of all the properties of the IndexingOptions class."
keywords: IndexingOptions, indexing options, cancellation, asynchronous indexing, indexing threads, separate process, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains a description of all the properties of the [IndexingOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/) class. The [UpdateOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/updateoptions/) class used for [updating an index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md" >}}) has the same properties. The properties that both classes inherit from the [BaseOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/) class are linked to that class.

## cancellation property

The **[cancellation](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/cancellation/)** property is used to specify an object of type [Cancellation](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/cancellation/), used to cancel an index operation. The default value of the [cancellation](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/cancellation/) property is `None`. An operation can be cancelled at once with the [cancel](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/cancellation/cancel/) method or after a time period with the [cancel_after](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/cancellation/cancel_after/) method. Note that after cancellation, an index may stop an operation with some delay.

A document whose indexing was interrupted by cancellation remains in the index marked with the [indexed_with_error](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/indexed_with_error/) flag. The next [update]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md" >}}) of the index indexes such a document again. Documents added from streams, structures or extracted data cannot be indexed again by an update and have to be added again.

The following example demonstrates cancelling of an indexing operation.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.common import Cancellation
from groupdocs.search.options import IndexingOptions

# Creating an index in the specified folder
index = Index("./index")

# Creating an instance of indexing options
options = IndexingOptions()
options.cancellation = Cancellation()  # Setting a cancellation object
options.cancellation.cancel_after(300000)  # Setting a time period of 300 seconds after which the indexing operation will be cancelled

# Indexing documents from the specified folder
index.add("./documents", options)

print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Newton.txt), [documents/Lecture.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Lecture.docx), [documents/Biography.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Biography.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "cancellation-property.txt" >}}  
```text
Indexed documents: 4
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-options/cancellation_property/cancellation-property.txt)
{{< /tab >}}
{{< /tabs >}}

## is_async property

The **[is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/is_async/)** property is used to specify whether to perform indexing operation asynchronously or synchronously. The default value is `False`, meaning synchronous execution. In asynchronous mode, the event handlers run on engine worker threads, and the index can be used again when the [operation_finished](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/operation_finished/) event is raised. The example below demonstrates the performing of an asynchronous indexing: a `threading.Event` set by the handler of the operation_finished event tells the main thread that the operation is completed.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import threading

from groupdocs.search import Index
from groupdocs.search.common import IndexStatus
from groupdocs.search.options import IndexingOptions

# Creating index in the specified folder
index = Index("./index")


# Subscribing to the event
def on_status_changed(sender, args):
    if args.status == IndexStatus.READY or args.status == IndexStatus.FAILED:
        # A notification of the operation completion should be here
        print("Index status:", args.status.name)


index.events.status_changed += on_status_changed

# The operation_finished event is raised when the operation is completed
finished = threading.Event()
index.events.operation_finished += lambda sender, args: finished.set()

# Creating an instance of indexing options
options = IndexingOptions()
options.is_async = True  # Specifying the asynchronous performing of the operation

# Indexing documents from the specified folder
# The method will return control before the indexing operation is completed
index.add("./documents", options)

# Waiting for the operation to complete before using the index
finished.wait()
print("Documents found:", index.search("Einstein").document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Newton.txt), [documents/Lecture.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Lecture.docx), [documents/Biography.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Biography.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "is-async-property.txt" >}}  
```text
Index status: READY
Documents found: 3
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-options/is_async_property/is-async-property.txt)
{{< /tab >}}
{{< /tabs >}}

## auto_detect_encoding property

The [auto_detect_encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/auto_detect_encoding/) property is a flag that allows you to automatically detect the following encodings of text files during indexing: UTF-32 LE, UTF-32 BE, UTF-16 LE, UTF-16 BE, UTF-8, UTF-7. By default, the encoding auto detection of text files is disabled. But in any case, the encoding of a text file can be set during indexing when the [file_indexing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/file_indexing/) event is raised. Detailed information on detecting and setting the encoding of text files is presented on the page [Text file encoding detection]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/text-file-encoding-detection.md" >}}).

## encoding property

The [encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/encoding/) property specifies the encoding used to extract text from text files. The default value is `None`, which means that the default encoding UTF-8 is used. If [auto_detect_encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/auto_detect_encoding/) is `True`, the value of this property is used as the default encoding.

## threads property

The [threads](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/threads/) property is used to set the number of threads used for indexing. The default value is 1. The number of indexing threads cannot be less than 1 or more than 4.

A larger number of threads accelerates the indexing process. However, be careful when specifying the number of indexing threads more than 1, because the amount of memory used during indexing is directly proportional to the number of threads involved. If there is 4 GB of RAM or less in the system, using more than 1 thread may result in out of memory error. In addition, the hard disk where the files for indexing are located may be the bottleneck and performance will not increase.

The following example demonstrates how to perform indexing in 2 threads.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import IndexingOptions

# Creating an index in the specified folder
index = Index("./index")

# Creating an instance of indexing options
options = IndexingOptions()
options.threads = 2  # Setting the number of indexing threads

# Indexing documents from the specified folder
index.add("./documents", options)

print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Newton.txt), [documents/Lecture.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Lecture.docx), [documents/Biography.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Biography.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "threads-property.txt" >}}  
```text
Indexed documents: 4
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-options/threads_property/threads-property.txt)
{{< /tab >}}
{{< /tabs >}}

## separate_process_options property

The [separate_process_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/separate_process_options/) property is represented by an object of the [SeparateProcessOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/) class. This class contains properties for extracting data from documents in a separate process:

* The [extract_in_separate_process](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/extract_in_separate_process/) property sets a value indicating whether a separate process is used to extract data from documents. The default value is `False`. When it is enabled, data are extracted in one thread, and the [threads](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/threads/) property is ignored.
* The [timeout](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/timeout/) property sets the maximum time to wait for the result of data extraction in a separate process. The default value is 10 minutes.
* The [assembly_path](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/assembly_path/) property sets the path to the assembly of the separate process.
* The [use_dotnet_to_start_process](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/use_dotnet_to_start_process/) property sets a value indicating whether `dotnet` is used to start the process. The default value is `True`. Set it to `False` to start an executable file directly; a Python extraction service is started this way, through a launcher.

Detailed information and an example are presented on the page [Extraction in separate process]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/extraction-in-separate-process.md" >}}).

## use_raw_text_extraction property

The [use_raw_text_extraction](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/use_raw_text_extraction/) property allows you to specify a value indicating whether to use raw text extraction mode whenever possible during indexing. The raw text extraction mode can significantly speed up the indexing process at the cost of losing the formatting quality of the extracted text. The default value for this property is `False`.

## use_backup_privilege property

The [use_backup_privilege](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/use_backup_privilege/) property allows you to specify whether the process should use the backup privilege to access files and directories. When set to `True`, this property allows the process to bypass file and directory security checks, permitting access to protected resources typically restricted by ACLs, as long as the process has the proper privileges. Use this property with caution, as it may allow access to sensitive system files and user data.

This feature is only supported on Windows operating systems. On non-Windows platforms,
enabling this property has no effect.

Note that this requires the process to be running with administrative rights.

## metadata_indexing_options property

The [metadata_indexing_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/metadata_indexing_options/) property is represented by an object of the [MetadataIndexingOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/) class. This class contains properties for setting metadata indexing options:

* The [indexing_empty_values](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/indexing_empty_values/) property sets a value indicating whether to index empty field values or not. The default value is `True`.
* The [indexing_empty_names](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/indexing_empty_names/) property sets a value indicating whether to index empty field names or not. The default value is `True`.
* The [default_field_name](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/default_field_name/) property sets the default field name used to index empty field names. The default value is "unknown".
* The [separator_in_compound_name](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/separator_in_compound_name/) property sets the separator in the compound name of a field. The default value is "." (period character).
* The [max_bytes_to_index_field](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/max_bytes_to_index_field/) property sets the maximum number of values indexed from an array of byte values. The default value is 2147483647 (the maximum 32-bit integer).
* The [max_ints_to_index_field](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/max_ints_to_index_field/) property sets the maximum number of values indexed from an array of int values. The default value is 2147483647 (the maximum 32-bit integer).
* The [max_longs_to_index_field](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/max_longs_to_index_field/) property sets the maximum number of values indexed from an array of long values. The default value is 2147483647 (the maximum 32-bit integer).
* The [max_doubles_to_index_field](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/max_doubles_to_index_field/) property sets the maximum number of values indexed from an array of double values. The default value is 2147483647 (the maximum 32-bit integer).
* The [separator_between_values](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/metadataindexingoptions/separator_between_values/) property sets the separator between values in a field of type array. The default value is " " (space character).

The following example demonstrates how to set the metadata indexing options.

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import IndexingOptions

# Creating an index
index = Index("./index")

# Setting the metadata indexing options
options = IndexingOptions()
options.metadata_indexing_options.default_field_name = "default"
options.metadata_indexing_options.separator_in_compound_name = "\\"
options.metadata_indexing_options.max_bytes_to_index_field = 10
options.metadata_indexing_options.max_ints_to_index_field = 10
options.metadata_indexing_options.max_longs_to_index_field = 10
options.metadata_indexing_options.max_doubles_to_index_field = 10

# Starting indexing operation
index.add("./documents", options)

print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Newton.txt), [documents/Lecture.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Lecture.docx), [documents/Biography.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-options/documents/Biography.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "metadata-indexing-options-property.txt" >}}  
```text
Indexed documents: 4
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-options/metadata_indexing_options_property/metadata-indexing-options-property.txt)
{{< /tab >}}
{{< /tabs >}}

## ocr_indexing_options property

The [ocr_indexing_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/ocr_indexing_options/) property is represented by an object of the [OcrIndexingOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/ocrindexingoptions/) class. This class contains properties for setting OCR processing options:

* The [enabled_for_separate_images](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/ocrindexingoptions/enabled_for_separate_images/) property sets a value indicating whether to recognize text in separate image files. The default value is `False`.
* The [enabled_for_embedded_images](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/ocrindexingoptions/enabled_for_embedded_images/) property sets a value indicating whether to recognize text in embedded images. The default value is `False`.
* The [enabled_for_container_item_images](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/ocrindexingoptions/enabled_for_container_item_images/) property sets a value indicating whether to recognize text in images that are items in a container (ZIP archive, OST/PST storage). The default value is `False`.
* The [ocr_connector](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/ocrindexingoptions/ocr_connector/) property sets an OCR connector that is used for OCR processing. The default value is `None`, which means no OCR is used.

## image_indexing_options property

The [image_indexing_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/image_indexing_options/) property is represented by an object of the [ImageIndexingOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imageindexingoptions/) class. This class provides image indexing options for reverse image search:

* The [enabled_for_separate_images](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imageindexingoptions/enabled_for_separate_images/) property sets a value indicating whether to index separate image files. The default value is `False`.
* The [enabled_for_embedded_images](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imageindexingoptions/enabled_for_embedded_images/) property sets a value indicating whether to index embedded images (for example, images in a DOCX document). The default value is `False`.
* The [enabled_for_container_item_images](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imageindexingoptions/enabled_for_container_item_images/) property sets a value indicating whether to index images that are items in a container (ZIP archive, OST/PST storage). The default value is `False`.

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
