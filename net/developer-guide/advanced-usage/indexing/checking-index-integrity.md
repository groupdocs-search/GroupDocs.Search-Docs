---
id: checking-index-integrity
url: search/net/checking-index-integrity
title: Checking index integrity
weight: 3
description: "This article describes how GroupDocs.Search protects index data, how an interrupted operation is recovered, and how to check and repair a damaged index with the CheckSegments method."
keywords: index integrity, check index, repair index, damaged index, CheckSegments, checksums, recovery
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page describes how GroupDocs.Search protects the data of an index on disk, how an index recovers after an interrupted operation, and how to check and repair a damaged index.

## Protection of index data

Since version 26.10, the index data are protected with checksums, so damaged data are detected when they are read and are reported instead of being used. All changes to an index are applied atomically: if an operation is interrupted by an error, a power failure or termination of the process, the index remains in its previous consistent state, and the changes of the interrupted operation are not visible.

An indexing operation writes the new data to separate files first and then merges them with the index. If the operation is interrupted after its data have been written, the next time the index is opened:

*   the written data are verified and merged with the index, so the indexing operation is completed;
*   if the written data are damaged or incomplete, they are discarded, so the indexing operation is rolled back and the index keeps its state before the operation. The changes of the operation are lost and have to be made again, for example, by repeating the interrupted operation. Since this happens while the index is being opened, the reason is written to the [logger]({{< ref "search/net/developer-guide/advanced-usage/indexing/logging.md" >}}) of the index, if it is specified in the index settings passed to the constructor.

A document whose indexing was interrupted by [cancellation]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-options.md#cancellation-property" >}}) is marked with the [IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/) flag and is indexed again by the next [update]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md" >}}) of the index.

## Checking and repairing an index

The [CheckSegments](https://reference.groupdocs.com/search/net/groupdocs.search/index/checksegments/) method of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class checks the files of an index, for example, after a disk failure or when errors about damaged index files are reported. The method verifies the checksum of every file of the index, including the term dictionary of the index, the posting lists, the files of every segment and the files of the stored document texts, and also verifies that the data of the segments match the data of the whole index.

Every damage found and every repair made is reported as an index error: through the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event and to the [logger]({{< ref "search/net/developer-guide/advanced-usage/indexing/logging.md" >}}) of the index, if it is specified. If no errors are reported, the index is intact.

The parameter of the method specifies whether a damaged index is repaired:

*   **false** - the index is only checked, and nothing is changed;
*   **true** - the index is checked, and if damage is found, the index is repaired.

The repair keeps as many documents as possible:

*   A damaged file of the index data that is kept in memory, such as the index metadata or a dictionary, is written again.
*   A damaged term dictionary of a segment is restored from the data of the whole index, and the term dictionary and the posting lists of the whole index are restored from the data of the segments.
*   A segment whose postings are damaged, or whose term dictionary cannot be restored, is excluded from the index, and its documents are deleted.
*   A damaged stored text of a document is removed, and the document is deleted. If the table of a text file is damaged, the file is excluded together with the documents of its texts.

The repaired index is saved at once. The documents deleted by the repair are reported in the messages of the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event. The paths of the indexed files and folders are kept, so the next [update]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md" >}}) of the index indexes the files of the deleted documents again. Documents added from streams, structures or extracted data cannot be indexed again by an update and have to be added again.

Checking segments is an index operation: the index gets the [CheckingSegments](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexstatus/) status while it is performed. If the index is performing another operation, the method does nothing and reports the reason through the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event. Searches are not blocked by the check: they work either with the damaged or with the repaired index.

The following example demonstrates how to check an index and repair it if damage is found.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an existing index
Index index = new Index(indexFolder);

// Collecting the messages about damage and repair
List<string> messages = new List<string>();
index.Events.ErrorOccurred += (sender, args) =>
{
    messages.Add(args.Message);
};

// Checking the index without changing it
index.CheckSegments(false);

if (messages.Count > 0)
{
    foreach (string message in messages)
    {
        Console.WriteLine(message);
    }

    // Repairing the index
    messages.Clear();
    index.CheckSegments(true);
    foreach (string message in messages)
    {
        Console.WriteLine(message);
    }

    // Indexing again the files of the documents deleted by the repair
    index.Update();
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
