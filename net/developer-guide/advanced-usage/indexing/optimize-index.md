---
id: optimize-index
url: search/net/optimize-index
title: Optimize index
weight: 22
description: "This article explains how to optimize an index to reduce the number of its segments and physically remove deleted documents."
keywords: optimize index, index segments, merge segments, remove deleted documents
productName: GroupDocs.Search for .NET
hideChildren: False
---
An index optimization operation allows you to reduce the number of segments in an index, thereby increasing search performance in the index. Each indexing operation adds at least one new segment to an index, so an index to which documents are often added should be optimized from time to time.

The optimization also physically removes [deleted documents]({{< ref "search/net/developer-guide/advanced-usage/indexing/delete-indexed-documents.md" >}}) from the index. Deleting documents does not rewrite the index: deleted documents are excluded from search immediately, but their data remain in the index files until the index is optimized.

The memory consumption of the optimization does not depend on the index size: a very large index is optimized into several segments of limited size, so the [SegmentCount](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexinfo/segmentcount/) property of [Index.IndexInfo](https://reference.groupdocs.com/search/net/groupdocs.search/index/indexinfo/) may be greater than 1 after optimizing such an index.

To perform this operation, call the parameterless [Optimize](https://reference.groupdocs.com/search/net/groupdocs.search/index/optimize/) method or the overload that takes the [MergeOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/mergeoptions/) instance as a parameter. In the [MergeOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/mergeoptions/) class, there is a property for specifying a cancellation object and a property for specifying whether to perform the operation asynchronously. By default, the operation is performed synchronously.

Since version 26.11, the cancellation stops every stage of the optimization, also the last one, which merges the terms of all the segments of the index and takes minutes on a large index; a cancelled optimization keeps the index as it was. Also since version 26.11, a [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/) object can be disposed of, for example by a using statement, to release the timer of [CancelAfter](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/cancelafter/), as the example below shows. The progress of the optimization is reported by the [OptimizationProgressChanged](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/optimizationprogresschanged/) event: since version 26.11, the last event, whose ProcessedSegments equals TotalSegments, comes when the optimization is done, before the optimized index is saved; before, it came before the last stage. The end of the operation is reported by the [OperationFinished](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/operationfinished/) event.

Since version 26.11, the optimization verifies the checksums of the index data that it reads. An optimization that reads damaged data fails: the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event reports that a file of the index is damaged, the files written by the optimization are deleted, and the index keeps its state. Repair the index with the [CheckSegments](https://reference.groupdocs.com/search/net/groupdocs.search/index/checksegments/) method and repeat the optimization (see [Checking index integrity]({{< ref "search/net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}})).

The following example demonstrates how to perform the index optimization.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder1 = @"c:\MyDocuments1\";
string documentsFolder2 = @"c:\MyDocuments2\";
string documentsFolder3 = @"c:\MyDocuments3\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
index.Add(documentsFolder1); // Indexing documents from the specified folder
index.Add(documentsFolder2); // Each call to Add creates at least one new segment in the index
index.Add(documentsFolder3);
 
// Creating cancellation object to be able to cancel the operation; disposing of it releases its timer
using (Cancellation cancellation = new Cancellation())
{
    MergeOptions options = new MergeOptions();
    options.Cancellation = cancellation;
    cancellation.CancelAfter(100000); // Setting maximum duration of the operation to 100 seconds
 
    // Merging segments of the index
    index.Optimize(options);
}

Console.WriteLine("Segment count: " + index.IndexInfo.SegmentCount);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
