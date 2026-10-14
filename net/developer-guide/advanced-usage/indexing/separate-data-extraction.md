---
id: separate-data-extraction
url: search/net/separate-data-extraction
title: Separate data extraction
weight: 23
description: "This article explains how to separately extract data from documents and add the extracted data to the index."
keywords: separate data extraction, Extractor, ExtractedData, ExtractionOptions, serialize extracted data
productName: GroupDocs.Search for .NET
hideChildren: False
---
The feature described in this article makes it possible to separate the operations of extracting data from a document and adding the extracted data to the index. The extracted data can be easily serialized and deserialized as needed.

This feature can be useful because the operation of extracting text and other data from documents can be very long for some formats. At the same time, adding data to the index is a rather lightweight operation. Therefore, separating these operations significantly reduces the time during which the index is busy performing operations, and adding more data extractors increases the overall indexing performance. Data extractors can even work on separate servers, completely freeing the indexing server from extracting operations.

The [Extractor](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/) class is used to extract data from documents. Upon completion of the operation, the [Extract](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/extract/) method returns an instance of the [ExtractedData](https://reference.groupdocs.com/search/net/groupdocs.search.common/extracteddata/) class, which is used directly to add to the index.

The [Extractor](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/) class has the following members in addition to the [Extract](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/extract/) method:

*   The [Settings](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/settings/) property returns an [ExtractorSettings](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractorsettings/) object. Its [IndexType](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractorsettings/indextype/) property specifies the type of the index for which data are extracted (the default value is NormalIndex); set it to the type of the index to which the data will be added. Its [TempFolder](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractorsettings/tempfolder/) property specifies the folder for temporary files.
*   The [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/erroroccurred/), [PasswordRequired](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/passwordrequired/) and [ImagePreparing](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/imagepreparing/) events, which work like the [index events]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}) with the same names.

The [ExtractionOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/extractionoptions/) class contains the same extraction options as the [IndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/) class inherited from [BaseOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/) (see [Indexing options]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-options.md" >}})), as well as the [CustomExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.options/extractionoptions/customextractor/) property, which specifies a [custom text extractor]({{< ref "search/net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}) for the document.

The following example demonstrates how to perform separate data extraction and indexing.

**C#**

```csharp
string indexFolder = @"c:\MyIndex";
string documentPath = @"c:\MyDocuments\MyDocument.pdf";

// Extracting data from a document
Extractor extractor = new Extractor();
extractor.ErrorOccurred += (sender, args) => Console.WriteLine(args.Message);
Document document = Document.CreateFromFile(documentPath);
ExtractionOptions extractionOptions = new ExtractionOptions();
extractionOptions.UseRawTextExtraction = false;
ExtractedData extractedData = extractor.Extract(document, extractionOptions);

// Serializing the data
byte[] array = extractedData.Serialize();

// Deserializing the data
ExtractedData deserializedData = ExtractedData.Deserialize(array);

// Creating an index
Index index = new Index(indexFolder);

if (deserializedData.IsExtractionSuccessful)
{
    // Indexing the data
    ExtractedData[] data = new ExtractedData[]
    {
        deserializedData
    };
    index.Add(data, new IndexingOptions());

    // Searching in the index
    SearchResult result = index.Search("Einstein");
}
```

Note that when indexed documents change and need to be updated in the index, the same code must be executed. That is, data must be extracted separately from the updated documents and then the extracted data must be added to the index. The [Update](https://reference.groupdocs.com/search/net/groupdocs.search/index/update/) method does not update documents added from extracted data.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
