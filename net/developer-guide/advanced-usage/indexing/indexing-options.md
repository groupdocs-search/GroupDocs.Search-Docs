---
id: indexing-options
url: search/net/indexing-options
title: Indexing options
weight: 12
description: "This page contains a description of all the properties of the IndexingOptions class"
keywords: IndexingOptions
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains a description of all the properties of the [IndexingOptions](https://reference.groupdocs.com/net/search/groupdocs.search.options/indexingoptions) class.

## Cancellation property

The **[Cancellation](https://reference.groupdocs.com/net/search/groupdocs.search.options/indexingoptions/properties/cancellation)** property is used to specify an object of type [Cancellation](https://reference.groupdocs.com/net/search/groupdocs.search.common/cancellation), used to cancel of an index operation. The default value of the [Cancellation](https://reference.groupdocs.com/net/search/groupdocs.search.options/indexingoptions/properties/cancellation) property is null. An operation can be canceled if necessary with a time delay. Note that after cancellation, an index may not immediately stop an operation, but with a some delay.

The following example demonstrates cancelling of an indexing operation.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Creating an instance of indexing options
IndexingOptions options = new IndexingOptions();
options.Cancellation = new Cancellation(); // Setting a cancellation object
options.Cancellation.CancelAfter(300000); // Setting a time period of 300 seconds after which the indexing operation will be cancelled
 
// Indexing documents from the specified folder
index.Add(documentFolder, options);
```

## IsAsync property

The **[IsAsync](https://reference.groupdocs.com/net/search/groupdocs.search.options/indexingoptions/properties/isasync)** property is used to specify whether to perform indexing operation asynchronously or synchronously. The default value is false, meaning synchronous execution. The example below demonstrates the performing of an asynchronous indexing.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating index in the specified folder
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.StatusChanged += (sender, args) =>
{
    if (args.Status != IndexStatus.InProgress)
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

## Threads property

The [Threads](https://reference.groupdocs.com/net/search/groupdocs.search.options/indexingoptions/properties/threads) property is used to set the number of threads used for indexing. The default value is 1. The number of indexing threads cannot be less than 1 or more than 4.

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

## MetadataIndexingOptions property

The [MetadataIndexingOptions](https://reference.groupdocs.com/net/search/groupdocs.search.options/indexingoptions/properties/metadataindexingoptions) property is represented by an object of the [MetadataIndexingOptions](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions) class. This class contains properties for setting metadata indexing options:

* The [IndexingEmptyValues](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions/properties/indexingemptyvalues) property sets a value indicating whether to index empty field values or not. The default value is true.
* The [IndexingEmptyNames](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions/properties/indexingemptynames) property sets a value indicating whether to index empty field names or not. The default value is true.
* The [DefaultFieldName](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions/properties/defaultfieldname) property sets the default field name used to index empty field names. The default value is "unknown".
* The [SeparatorInCompoundName](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions/properties/separatorincompoundname) property sets the separator in the compound name of a field. The default value is "." (period character).
* The [MaxBytesToIndexField](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions/properties/maxbytestoindexfield) property sets the maximum number of values indexed from an array of byte values. The default value is int.MaxValue.
* The [MaxIntsToIndexField](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions/properties/maxintstoindexfield) property sets the maximum number of values indexed from an array of int values. The default value is int.MaxValue.
* The [MaxLongsToIndexField](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions/properties/maxlongstoindexfield) property sets the maximum number of values indexed from an array of long values. The default value is int.MaxValue.
* The [MaxDoublesToIndexField](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions/properties/maxdoublestoindexfield) property sets the maximum number of values indexed from an array of double values. The default value is int.MaxValue.
* The [SeparatorBetweenValues](https://reference.groupdocs.com/net/search/groupdocs.search.options/metadataindexingoptions/properties/separatorbetweenvalues) property sets the separator between values in a field of type array. The default value is " " (space character).

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

The [OcrIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/properties/ocrindexingoptions) property is represented by an object of the [OcrIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions) class. This class contains properties for setting OCR processing options:

* The [EnabledForSeparateImages](https://reference.groupdocs.com/net/search/groupdocs.search.options/ocrindexingoptions/properties/enabledforseparateimages) property sets a value indicating whether to recognize text in separate image files. The default value is false.
* The [EnabledForEmbeddedImages](https://reference.groupdocs.com/net/search/groupdocs.search.options/ocrindexingoptions/properties/enabledforembeddedimages) property sets a value indicating whether to recognize text in embedded images. The default value is false.
* The [EnabledForContainerItemImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions/properties/enabledforcontaineritemimages) property sets a value indicating whether to recognize text in images that are items in a container (ZIP archive, OST/PST storage). The default value is false.
* The [OcrConnector](https://reference.groupdocs.com/net/search/groupdocs.search.options/ocrindexingoptions/properties/ocrconnector) property sets an OCR connector that is used for OCR processing. The default value is null, which means no OCR is used.

## ImageIndexingOptions property

The [ImageIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/properties/imageindexingoptions) property is represented by an object of the [ImageIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/imageindexingoptions) class. This class provides image indexing options for reverse image search:

* The [EnabledForSeparateImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/imageindexingoptions/properties/enabledforseparateimages) property sets a value indicating whether to index separate image files. The default value is false.
* The [EnabledForEmbeddedImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/imageindexingoptions/properties/enabledforembeddedimages) property sets a value indicating whether to index embedded images (for example, images in a DOCX document). The default value is false.
* The [EnabledForContainerItemImages](https://reference.groupdocs.com/search/net/groupdocs.search.options/imageindexingoptions/properties/enabledforcontaineritemimages) property sets a value indicating whether to index images that are items in a container (ZIP archive, OST/PST storage). The default value is false.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

* [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
* [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
