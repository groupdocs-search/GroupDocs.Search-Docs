---
id: indexing-options
url: search/net/indexing-options
title: Indexing options
weight: 15
description: "This page contains a description of all the properties of the IndexingOptions class."
keywords: IndexingOptions, indexing options, cancellation, asynchronous indexing, indexing threads, separate process
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains a description of all the properties of the [IndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/) class. The [UpdateOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/updateoptions/) class used for [updating an index]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md" >}}) has the same properties. The properties that both classes inherit from the [BaseOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/) class are linked to that class.

{{< alert style="info" >}}Since version 26.11, a document whose data are too large for a segment of the index, about 2 GB of postings or about 500 million characters of distinct terms, fails with the error "The document is too large to be indexed: a segment of the index cannot hold the data of its terms.", which the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event reports. The terms of the document before the limit are indexed, the document is marked with the [IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/) flag, and the next documents are indexed. Split such a document, for example a log of several gigabytes, into smaller documents.{{< /alert >}}

## Cancellation property

The **[Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/cancellation/)** property is used to specify an object of type [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/), used to cancel an index operation. The default value of the [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/cancellation/) property is null. An operation can be cancelled at once with the [Cancel](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/cancel/) method or after a time period with the [CancelAfter](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/cancelafter/) method. Note that after cancellation, an index may stop an operation with some delay.

Since version 26.11, the [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/) class implements IDisposable: the [Dispose](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/dispose/) method releases the timer of a cancellation scheduled by [CancelAfter](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/cancelafter/), and the scheduled cancellation is not requested. Dispose of a cancellation, for example by a using statement, when the operation has finished: otherwise, its timer keeps the object, with the operations that it was given to, until the time comes. A cancellation that has been requested stays requested, and [CancelAfter](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/cancelafter/) throws ObjectDisposedException after Dispose.

A document whose indexing was interrupted by cancellation remains in the index marked with the [IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/) flag. The next [update]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md" >}}) of the index indexes such a document again. Documents added from streams, structures or extracted data cannot be indexed again by an update and have to be added again.

The following example demonstrates cancelling of an indexing operation.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Creating a cancellation object; disposing of it releases the timer of CancelAfter
using (Cancellation cancellation = new Cancellation())
{
    // Creating an instance of indexing options
    IndexingOptions options = new IndexingOptions();
    options.Cancellation = cancellation; // Setting a cancellation object
    cancellation.CancelAfter(300000); // Setting a time period of 300 seconds after which the indexing operation will be cancelled
 
    // Indexing documents from the specified folder
    index.Add(documentFolder, options);
}
```

## IsAsync property

The **[IsAsync](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/isasync/)** property is used to specify whether to perform indexing operation asynchronously or synchronously. The default value is false, meaning synchronous execution. The example below demonstrates the performing of an asynchronous indexing.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating index in the specified folder
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.StatusChanged += (sender, args) =>
{
    if (args.Status == IndexStatus.Ready || args.Status == IndexStatus.Failed)
    {
        // A notification of the operation completion should be here
    }
};
 
// Creating an instance of indexing options
IndexingOptions options = new IndexingOptions();
options.IsAsync = true; // Specifying the asynchronous performing of the operation
 
// Indexing documents from the specified folder
// The method will return control before the indexing operation is completed
index.Add(documentFolder, options);
```

## AutoDetectEncoding property

The [AutoDetectEncoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/autodetectencoding/) property is a flag that allows you to automatically detect the following encodings of text files during indexing: UTF-32 LE, UTF-32 BE, UTF-16 LE, UTF-16 BE, UTF-8, UTF-7. By default, the encoding auto detection of text files is disabled. But in any case, the encoding of a text file can be set during indexing when the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event is raised. Detailed information on detecting and setting the encoding of text files is presented on the page [Text file encoding detection]({{< ref "search/net/developer-guide/advanced-usage/indexing/text-file-encoding-detection.md" >}}).

## Encoding property

The [Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/encoding/) property specifies the encoding used to extract text from text files. The default value is null, which means that the default encoding UTF-8 is used. If [AutoDetectEncoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/autodetectencoding/) is true then value of this property is used as the default encoding. Since version 26.11, an HTML document without a byte order mark is read in this encoding or in the encoding that the FileIndexing event sets, if one is set, and otherwise in the charset that the document declares or in UTF-8 (see [Text file encoding detection]({{< ref "search/net/developer-guide/advanced-usage/indexing/text-file-encoding-detection.md#encoding-of-html-documents" >}})).

## Threads property

The [Threads](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/threads/) property is used to set the number of threads used for indexing. The default value is 1. The number of indexing threads cannot be less than 1 or more than 4.

A larger number of threads accelerates the indexing process. However, be careful when specifying the number of indexing threads more than 1, because the amount of memory used during indexing is directly proportional to the number of threads involved. If there is 4 GB of RAM or less in the system, using more than 1 thread may result in out of memory error. In addition, the hard disk where the files for indexing are located may be the bottleneck and performance will not increase.

The following example demonstrates how to perform indexing in 2 threads.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Creating an instance of indexing options
IndexingOptions options = new IndexingOptions();
options.Threads = 2; // Setting the number of indexing threads
 
// Indexing documents from the specified folder
index.Add(documentFolder, options); 
```

## SeparateProcessOptions property

The [SeparateProcessOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/separateprocessoptions/) property is represented by an object of the [SeparateProcessOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/) class. This class contains properties for extracting data from documents in a separate process:

* The [ExtractInSeparateProcess](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/extractinseparateprocess/) property sets a value indicating whether a separate process is used to extract data from documents. The default value is false. When it is enabled, data are extracted in one thread, and the [Threads](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/threads/) property is ignored.
* The [Timeout](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/timeout/) property sets the maximum time to wait for the result of data extraction in a separate process. The default value is 10 minutes. Since version 26.11, the separate process also has to connect to the index within this time after it is started.
* The [AssemblyPath](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/assemblypath/) property sets the path to the assembly of the separate process.
* The [UseDotnetToStartProcess](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/usedotnettostartprocess/) property sets a value indicating whether `dotnet` is used to start the process. The default value is true. Set it to false to start a self-contained application. On .NET Framework, `dotnet` is never used.

Detailed information and an example are presented on the page [Extraction in separate process]({{< ref "search/net/developer-guide/advanced-usage/indexing/extraction-in-separate-process.md" >}}).

## UseRawTextExtraction property

The [UseRawTextExtraction](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/userawtextextraction/) property allows you to specify a value indicating whether to use raw text extraction mode whenever possible during indexing. The raw text extraction mode can significantly speed up the indexing process at the cost of losing the formatting quality of the extracted text. The default value for this property is false.

## UseBackupPrivilege property

The [UseBackupPrivilege](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/usebackupprivilege/) property allows you to specify whether the process should use the backup privilege to access files and directories. When set to true, this property allows the process to bypass file and directory security checks, permitting access to protected resources typically restricted by ACLs, as long as the process has the proper privileges. Use this property with caution, as it may allow access to sensitive system files and user data.

This feature is only supported on Windows operating systems. On non-Windows platforms,
enabling this property has no effect.

Note that this requires the process to be running with administrative rights.

With this property, a file that cannot be opened is copied into a temporary file by the backup privilege and indexed from the copy, which is deleted after the extraction. An index on disk writes the copy to its folder; since version 26.11, an index in memory and an [Extractor](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/) without [ExtractorSettings.TempFolder](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractorsettings/tempfolder/) write it to the temporary folder of the system; before, their copies failed. Also since version 26.11:

* A file that cannot be copied either is indexed with an error: it is marked with the [IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/) flag, and its temporary copy is deleted. Before, the file was not recorded in the index.
* The privilege is enabled only while the operations that use it run, and it is disabled again after them if it was disabled before. An application that needs the privilege after an operation enables it itself.
* An account that does not hold the privilege is reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event when the operation starts, and the operation goes on.

## MetadataIndexingOptions property

The [MetadataIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/metadataindexingoptions/) property is represented by an object of the [MetadataIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/) class. This class contains properties for setting metadata indexing options:

* The [IndexingEmptyValues](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/indexingemptyvalues/) property sets a value indicating whether to index empty field values or not. The default value is true.
* The [IndexingEmptyNames](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/indexingemptynames/) property sets a value indicating whether to index empty field names or not. The default value is true.
* The [DefaultFieldName](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/defaultfieldname/) property sets the default field name used to index empty field names. The default value is "unknown".
* The [SeparatorInCompoundName](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/separatorincompoundname/) property sets the separator in the compound name of a field. The default value is "." (period character).
* The [MaxBytesToIndexField](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/maxbytestoindexfield/) property sets the maximum number of values indexed from an array of byte values. The default value is int.MaxValue.
* The [MaxIntsToIndexField](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/maxintstoindexfield/) property sets the maximum number of values indexed from an array of int values. The default value is int.MaxValue.
* The [MaxLongsToIndexField](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/maxlongstoindexfield/) property sets the maximum number of values indexed from an array of long values. The default value is int.MaxValue.
* The [MaxDoublesToIndexField](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/maxdoublestoindexfield/) property sets the maximum number of values indexed from an array of double values. The default value is int.MaxValue.
* The [SeparatorBetweenValues](https://reference.groupdocs.com/search/net/groupdocs.search.options/metadataindexingoptions/separatorbetweenvalues/) property sets the separator between values in a field of type array. The default value is " " (space character).

The following example demonstrates how to set the metadata indexing options.

**C#**

```csharp
string indexFolder = @"c:\MyIndex";
string documentFolder = @"c:\MyDocuments";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Setting the metadata indexing options
var options = new IndexingOptions();
options.MetadataIndexingOptions.DefaultFieldName = "default";
options.MetadataIndexingOptions.SeparatorInCompoundName = @"\";
options.MetadataIndexingOptions.MaxBytesToIndexField = 10;
options.MetadataIndexingOptions.MaxIntsToIndexField = 10;
options.MetadataIndexingOptions.MaxLongsToIndexField = 10;
options.MetadataIndexingOptions.MaxDoublesToIndexField = 10;
 
// Starting indexing operation
index.Add(documentFolder, options);
```

## OcrIndexingOptions property

The [OcrIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/ocrindexingoptions/) property is represented by an object of the [OcrIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions/) class. This class contains properties for setting OCR processing options:

* The [EnabledForSeparateImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions/enabledforseparateimages/) property sets a value indicating whether to recognize text in separate image files. The default value is false.
* The [EnabledForEmbeddedImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions/enabledforembeddedimages/) property sets a value indicating whether to recognize text in embedded images. The default value is false.
* The [EnabledForContainerItemImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions/enabledforcontaineritemimages/) property sets a value indicating whether to recognize text in images that are items in a container (ZIP archive, OST/PST storage). The default value is false.
* The [OcrConnector](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions/ocrconnector/) property sets an OCR connector that is used for OCR processing. The default value is null, which means no OCR is used. For the documents extracted in a separate process, the OCR connector of the extraction process is used instead (see [Extraction in separate process]({{< ref "search/net/developer-guide/advanced-usage/indexing/extraction-in-separate-process.md#recognition-of-text-in-images" >}})).

## ImageIndexingOptions property

The [ImageIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/imageindexingoptions/) property is represented by an object of the [ImageIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/imageindexingoptions/) class. This class provides image indexing options for reverse image search:

* The [EnabledForSeparateImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/imageindexingoptions/enabledforseparateimages/) property sets a value indicating whether to index separate image files. The default value is false.
* The [EnabledForEmbeddedImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/imageindexingoptions/enabledforembeddedimages/) property sets a value indicating whether to index embedded images (for example, images in a DOCX document). The default value is false.
* The [EnabledForContainerItemImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/imageindexingoptions/enabledforcontaineritemimages/) property sets a value indicating whether to index images that are items in a container (ZIP archive, OST/PST storage). The default value is false.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

* [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
* [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
