---
id: indexing-options
url: search/java/indexing-options
title: Indexing options
weight: 12
description: "This page contains a description of all the properties of the IndexingOptions class"
keywords: IndexingOptions
productName: GroupDocs.Search for Java
hideChildren: False
---
This page contains a description of all the properties of the [IndexingOptions](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/IndexingOptions) class.

## setCancellation method

The [setCancellation](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/IndexingOptions#setCancellation(com.groupdocs.search.common.Cancellation)) method is used to specify an object of type [Cancellation](https://reference.groupdocs.com/search/java/com.groupdocs.search.common/Cancellation), used to cancel of an index operation. The default value is null. An operation can be canceled if necessary with a time delay. Note that after cancellation, an index may not immediately stop an operation, but with a some delay.

The following example demonstrates cancelling of an indexing operation.

```java
String indexFolder = "c:\\MyIndex\\";
String documentFolder = "c:\\MyDocuments\\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Creating an instance of indexing options
IndexingOptions options = new IndexingOptions();
options.setCancellation(new Cancellation()); // Setting a cancellation object
options.getCancellation().cancelAfter(300000); // Setting a time period of 300 seconds after which the indexing operation will be cancelled

// Indexing documents from the specified folder
index.add(documentFolder, options);
```

## setAsync method

The [setAsync](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/IndexingOptions#setAsync(boolean)) method is used to specify whether to perform indexing operation asynchronously or synchronously. The default value is false, meaning synchronous execution. The example below demonstrates the performing of an asynchronous indexing.

```java
String indexFolder = "c:\\MyIndex\\";
String documentFolder = "c:\\MyDocuments\\";

// Creating index in the specified folder
Index index = new Index(indexFolder);

// Subscribing to the event
index.getEvents().StatusChanged.add(new EventHandler<BaseIndexEventArgs>() {
    public void invoke(Object sender, BaseIndexEventArgs args) {
        if (args.getStatus() != IndexStatus.InProgress) {
            // A notification of the operation completion should be here
        }
    }
});

// Creating an instance of indexing options
IndexingOptions options = new IndexingOptions();
options.setAsync(true); // Specifying the asynchronous performing of the operation

// Indexing documents from the specified folder
// The method will return control before the indexing operation is completed
index.add(documentFolder, options);
```

## setThreads method

The [setThreads](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/IndexingOptions#setThreads(int)) method is used to set the number of threads used for indexing. The default value is 1. The number of indexing threads cannot be less than 1 or more than 4.

A larger number of threads accelerates the indexing process. However, be careful when specifying the number of indexing threads more than 1, because the amount of memory used during indexing is directly proportional to the number of threads involved. If there is 4 GB of RAM or less in the system, using more than 1 thread may result in out of memory error. In addition, the hard disk where the files for indexing are located may be the bottleneck and performance will not increase.

The following example demonstrates how to perform indexing in 2 threads.

```java
String indexFolder = "c:\\MyIndex\\";
String documentFolder = "c:\\MyDocuments\\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Creating an instance of indexing options
IndexingOptions options = new IndexingOptions();
options.setThreads(2); // Setting the number of indexing threads

// Indexing documents from the specified folder
index.add(documentFolder, options);
```

## getMetadataIndexingOptions method

The [getMetadataIndexingOptions](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/IndexingOptions#getMetadataIndexingOptions()) method returns an instance of the [MetadataIndexingOptions](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions) class. This class contains methods for setting metadata indexing options:

* The [setIndexingEmptyValues](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions#setIndexingEmptyValues(boolean)) method sets a value indicating whether to index empty field values or not. The default value is true.
* The [setIndexingEmptyNames](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions#setIndexingEmptyNames(boolean)) method sets a value indicating whether to index empty field names or not. The default value is true.
* The [setDefaultFieldName](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions#setDefaultFieldName(java.lang.String)) method sets the default field name used to index empty field names. The default value is "unknown".
* The [setSeparatorInCompoundName](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions#setSeparatorInCompoundName(java.lang.String)) method sets the separator in the compound name of a field. The default value is "." (period character).
* The [setMaxBytesToIndexField](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions#setMaxBytesToIndexField(int)) method sets the maximum number of values indexed from an array of byte values. The default value is Integer.MAX\_VALUE.
* The [setMaxIntsToIndexField](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions#setMaxIntsToIndexField(int)) method sets the maximum number of values indexed from an array of int values. The default value is Integer.MAX\_VALUE.
* The [setMaxLongsToIndexField](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions#setMaxLongsToIndexField(int)) method sets the maximum number of values indexed from an array of long values. The default value is Integer.MAX\_VALUE.
* The [setMaxDoublesToIndexField](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions#setMaxDoublesToIndexField(int)) method sets the maximum number of values indexed from an array of double values. The default value is Integer.MAX\_VALUE.
* The [setSeparatorBetweenValues](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MetadataIndexingOptions#setSeparatorBetweenValues(java.lang.String)) method sets the separator between values in a field of type array. The default value is " " (space character).

The following example demonstrates how to set the metadata indexing options.

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Setting the metadata indexing options
IndexingOptions options = new IndexingOptions();
options.getMetadataIndexingOptions().setDefaultFieldName("default");
options.getMetadataIndexingOptions().setSeparatorInCompoundName("\\");
options.getMetadataIndexingOptions().setMaxBytesToIndexField(10);
options.getMetadataIndexingOptions().setMaxIntsToIndexField(10);
options.getMetadataIndexingOptions().setMaxLongsToIndexField(10);
options.getMetadataIndexingOptions().setMaxDoublesToIndexField(10);

// Starting indexing operation
index.add(documentFolder, options);
```

## getOcrIndexingOptions method

The [getOcrIndexingOptions](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/IndexingOptions#getOcrIndexingOptions()) method returns an instance of the [OcrIndexingOptions](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/OcrIndexingOptions) class. This class contains properties for setting OCR processing options:

* The [setEnabledForSeparateImages](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/OcrIndexingOptions#setEnabledForSeparateImages(boolean)) method sets a value indicating whether to recognize text in separate image files. The default value is false.
* The [setEnabledForEmbeddedImages](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/OcrIndexingOptions#setEnabledForEmbeddedImages(boolean)) method sets a value indicating whether to recognize text in embedded images. The default value is false.
* The [setEnabledForContainerItemImages](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/OcrIndexingOptions#setEnabledForContainerItemImages(boolean)) method sets a value indicating whether to recognize text in images that are items in a container (ZIP archive, OST/PST storage). The default value is false.
* The [setOcrConnector](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/OcrIndexingOptions#setOcrConnector(com.groupdocs.search.options.IOcrConnector)) method sets an OCR connector that is used for OCR processing. The default value is null, which means no OCR is used.

## getImageIndexingOptions method

The [getImageIndexingOptions](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/IndexingOptions#getImageIndexingOptions()) method returns an instance of the [ImageIndexingOptions](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/ImageIndexingOptions) class. This class provides image indexing options for reverse image search:

* The [setEnabledForSeparateImages](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/ImageIndexingOptions#setEnabledForSeparateImages(boolean)) method sets a value indicating whether to index separate image files. The default value is false.
* The [setEnabledForEmbeddedImages](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/ImageIndexingOptions#setEnabledForEmbeddedImages(boolean)) method sets a value indicating whether to index embedded images (for example, images in a DOCX document). The default value is false.
* The [setEnabledForContainerItemImages](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/ImageIndexingOptions#setEnabledForContainerItemImages(boolean)) method sets a value indicating whether to index images that are items in a container (ZIP archive, OST/PST storage). The default value is false.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

* [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

* [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
