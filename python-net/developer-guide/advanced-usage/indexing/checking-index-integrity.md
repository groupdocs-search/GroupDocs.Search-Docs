---
id: checking-index-integrity
url: search/python-net/checking-index-integrity
title: Checking index integrity
weight: 3
description: "This article describes how GroupDocs.Search protects index data, how an interrupted operation is recovered, and how to check and repair a damaged index with the check_segments method in Python."
keywords: index integrity, check index, repair index, damaged index, check_segments, checksums, recovery, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page describes how GroupDocs.Search protects the data of an index on disk, how an index recovers after an interrupted operation, and how to check and repair a damaged index.

## Protection of index data

Since version 26.10, the index data are protected with checksums, so damaged data are detected when they are read and are reported instead of being used. All changes to an index are applied atomically: if an operation is interrupted by an error, a power failure or termination of the process, the index remains in its previous consistent state, and the changes of the interrupted operation are not visible.

An indexing operation writes the new data to separate files first and then merges them with the index. If the operation is interrupted after its data have been written, the next time the index is opened:

*   the written data are verified and merged with the index, so the indexing operation is completed;
*   if the written data are damaged or incomplete, they are discarded, so the indexing operation is rolled back and the index keeps its state before the operation. The changes of the operation are lost and have to be made again, for example, by repeating the interrupted operation. Since this happens while the index is being opened, the reason is written to the [logger]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/logging.md" >}}) of the index, if it is specified in the index settings passed to the constructor.

A document whose indexing was interrupted by [cancellation]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-options.md#cancellation-property" >}}) is marked with the [indexed_with_error](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/indexed_with_error/) flag and is indexed again by the next [update]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md" >}}) of the index.

## Checking and repairing an index

The [check_segments](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/check_segments/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class checks the files of an index, for example, after a disk failure or when errors about damaged index files are reported. The method verifies the checksum of every file of the index, including the term dictionary of the index, the posting lists, the files of every segment and the files of the stored document texts, and also verifies that the data of the segments match the data of the whole index.

Every damage found and every repair made is reported as an index error: through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event and to the [logger]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/logging.md" >}}) of the index, if it is specified. If no errors are reported, the index is intact.

The parameter of the method specifies whether a damaged index is repaired:

*   **False** - the index is only checked, and nothing is changed;
*   **True** - the index is checked, and if damage is found, the index is repaired.

The repair keeps as many documents as possible:

*   A damaged file of the index data that is kept in memory, such as the index metadata or a dictionary, is written again.
*   A damaged term dictionary of a segment is restored from the data of the whole index, and the term dictionary and the posting lists of the whole index are restored from the data of the segments.
*   A segment whose postings are damaged, or whose term dictionary cannot be restored, is excluded from the index, and its documents are deleted.
*   A damaged stored text of a document is removed, and the document is deleted. If the table of a text file is damaged, the file is excluded together with the documents of its texts.

The repaired index is saved at once. The documents deleted by the repair are reported in the messages of the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event. The paths of the indexed files and folders are kept, so the next [update]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md" >}}) of the index indexes the files of the deleted documents again. Documents added from streams, structures or extracted data cannot be indexed again by an update and have to be added again.

Checking segments is an index operation: the index gets the [CHECKING_SEGMENTS](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/indexstatus/) status while it is performed. If the index is performing another operation, the method does nothing and reports the reason through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event. Searches are not blocked by the check: they work either with the damaged or with the repaired index.

The following example demonstrates how to check an index and repair it if damage is found. The messages are collected by a handler of the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event, which takes the sender and the event arguments.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Opening an existing index
index = Index("./index")

# Collecting the messages about damage and repair
messages = []
index.events.error_occurred += lambda sender, args: messages.append(args.message)

# Checking the index without changing it
index.check_segments(False)

if len(messages) > 0:
    for message in messages:
        print(message)

    # Repairing the index
    messages.clear()
    index.check_segments(True)
    for message in messages:
        print(message)

    # Indexing again the files of the documents deleted by the repair
    index.update()
else:
    print("No damage found: the index is intact.")
```
{{< /tab >}}
{{< tab "checking-and-repairing-an-index.txt" >}}  
```text
No damage found: the index is intact.
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/checking-index-integrity/checking_and_repairing_an_index/checking-and-repairing-an-index.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
