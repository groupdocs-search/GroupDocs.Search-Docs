---
id: groupdocs-search-for-net-22-10-release-notes
url: search/net/groupdocs-search-for-net-22-10-release-notes
title: GroupDocs.Search for .NET 22.10 Release Notes
weight: 10
description: ""
keywords: 
productName: GroupDocs.Search for .NET
hideChildren: False
---

{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for .NET 22.10{{< /alert >}}

## Major Features

There are the following features, enhancements, and fixes in this release:

- Implement reverse image search functionality
- Implement separate data extraction and indexing
- Implement support for .NET 6.0
- Implement increase in indexing speed
- Fix unsupported document format errors

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2666 | Implement reverse image search functionality | Feature |
| SEARCHNET-2668 | Implement separate data extraction and indexing | Feature |
| SEARCHNET-2650 | Implement support for .NET 6.0 | Enhancement |
| SEARCHNET-2690 | Implement increase in indexing speed | Enhancement |
| SEARCHNET-2710 | Fix unsupported document format errors | Fix |

## Public API and Backward Incompatible Changes

### Implement reverse image search functionality

The reverse image search functionality provides the ability to search for similar images in archives, documents, and individual files. For more information, see the [Reverse image search](https://docs.groupdocs.com/search/net/reverse-image-search/) documentation page.

##### Public API changes

Class **DocumentImage** has been added to **GroupDocs.Search.Common** namespace.  
Property **GroupDocs.Search.Common.DocumentField[] Fields** has been added to **GroupDocs.Search.Common.DocumentImage** class.  
Property **GroupDocs.Search.Common.ImageFrame[] Frames** has been added to **GroupDocs.Search.Common.DocumentImage** class.  
Property **Int32 ImageIndex** has been added to **GroupDocs.Search.Common.DocumentImage** class.

Class **FoundImageFrame** has been added to **GroupDocs.Search.Results** namespace.  
Property **GroupDocs.Search.Results.DocumentInfo DocumentInfo** has been added to **GroupDocs.Search.Results.FoundImageFrame** class.  
Property **Int32 FrameIndex** has been added to **GroupDocs.Search.Results.FoundImageFrame** class.  
Property **Int32 HashDifferences** has been added to **GroupDocs.Search.Results.FoundImageFrame** class.  
Property **Int32 ImageIndex** has been added to **GroupDocs.Search.Results.FoundImageFrame** class.

Class **ImageFrame** has been added to **GroupDocs.Search.Common** namespace.  
Property **Int32 Index** has been added to **GroupDocs.Search.Common.ImageFrame** class.

Class **ImageIndexingOptions** has been added to **GroupDocs.Search.Options** namespace.  
Property **Boolean EnabledForContainerItemImages** has been added to **GroupDocs.Search.Options.ImageIndexingOptions** class.  
Property **Boolean EnabledForEmbeddedImages** has been added to **GroupDocs.Search.Options.ImageIndexingOptions** class.  
Property **Boolean EnabledForSeparateImages** has been added to **GroupDocs.Search.Options.ImageIndexingOptions** class.

Class **ImagePreparingEventArgs** has been added to **GroupDocs.Search.Events** namespace.  
Property **System.String DocumentKey** has been added to **GroupDocs.Search.Events.ImagePreparingEventArgs** class.  
Property **GroupDocs.Search.Common.ImageFrame[] ImageFrames** has been added to **GroupDocs.Search.Events.ImagePreparingEventArgs** class.  
Property **Int32 ImageIndex** has been added to **GroupDocs.Search.Events.ImagePreparingEventArgs** class.  
Property **System.IO.Stream ImageStream** has been added to **GroupDocs.Search.Events.ImagePreparingEventArgs** class.  
Property **System.String[] InnerPath** has been added to **GroupDocs.Search.Events.ImagePreparingEventArgs** class.

Class **ImageSearchOptions** has been added to **GroupDocs.Search.Options** namespace.  
Property **Int32 HashDifferences** has been added to **GroupDocs.Search.Options.ImageSearchOptions** class.  
Property **Int32 MaxResultCount** has been added to **GroupDocs.Search.Options.ImageSearchOptions** class.  
Property **GroupDocs.Search.Options.ISearchDocumentFilter SearchDocumentFilter** has been added to **GroupDocs.Search.Options.ImageSearchOptions** class.

Class **ImageSearchResult** has been added to **GroupDocs.Search.Results** namespace.  
Method **GroupDocs.Search.Results.FoundImageFrame GetFoundImage(Int32)** has been added to **GroupDocs.Search.Results.ImageSearchResult** class.  
Property **Int32 ImageCount** has been added to **GroupDocs.Search.Results.ImageSearchResult** class.

Class **SearchImage** has been added to **GroupDocs.Search.Common** namespace.  
Method **GroupDocs.Search.Common.SearchImage Create(System.String)** has been added to **GroupDocs.Search.Common.SearchImage** class.  
Method **GroupDocs.Search.Common.SearchImage Create(System.String, Int32)** has been added to **GroupDocs.Search.Common.SearchImage** class.  
Method **GroupDocs.Search.Common.SearchImage Create(System.IO.Stream)** has been added to **GroupDocs.Search.Common.SearchImage** class.  
Method **GroupDocs.Search.Common.SearchImage Create(System.IO.Stream, Int32)** has been added to **GroupDocs.Search.Common.SearchImage** class.

Property **GroupDocs.Search.Options.ImageIndexingOptions ImageIndexingOptions** has been added to **GroupDocs.Search.Options.IndexingOptions** class.  
Property **GroupDocs.Search.Options.ImageIndexingOptions ImageIndexingOptions** has been added to **GroupDocs.Search.Options.TextOptions** class.  
Property **GroupDocs.Search.Options.ImageIndexingOptions ImageIndexingOptions** has been added to **GroupDocs.Search.Options.UpdateOptions** class.

Event **System.EventHandler<GroupDocs.Search.Events.ImagePreparingEventArgs> ImagePreparing** has been added to **GroupDocs.Search.Events.EventHub** class.

Property **Boolean EnabledForContainerItemImages** has been added to **GroupDocs.Search.Options.OcrIndexingOptions** class.

Method **GroupDocs.Search.Results.ImageSearchResult Search(GroupDocs.Search.Common.SearchImage, GroupDocs.Search.Options.ImageSearchOptions)** has been added to **GroupDocs.Search.Index** class.

##### Use cases

The following example demonstrates how to perform reverse image search in an index.

```csharp
string indexFolder = @"c:\MyIndex";
string documentFolder = @"c:\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Setting the image indexing options
IndexingOptions indexingOptions = new IndexingOptions();
indexingOptions.ImageIndexingOptions.EnabledForContainerItemImages = true;
indexingOptions.ImageIndexingOptions.EnabledForEmbeddedImages = true;
indexingOptions.ImageIndexingOptions.EnabledForSeparateImages = true;

// Indexing documents in a document folder
index.Add(documentFolder, indexingOptions);

// Setting the image search options
ImageSearchOptions imageSearchOptions = new ImageSearchOptions();
imageSearchOptions.HashDifferences = 10;
imageSearchOptions.MaxResultCount = 100;
imageSearchOptions.SearchDocumentFilter = SearchDocumentFilter.CreateFileExtension(".zip", ".png", ".jpg");

// Creating a reference image for search
SearchImage searchImage = SearchImage.Create(@"c:\MyDocuments\image.png");

// Searching in the index
ImageSearchResult result = index.Search(searchImage, imageSearchOptions);

Console.WriteLine("Images found: " + result.ImageCount);
for (int i = 0; i < result.ImageCount; i++)
{
    FoundImageFrame image = result.GetFoundImage(i);
    Console.WriteLine(image.DocumentInfo.ToString());
}
```

### Implement separate data extraction and indexing

The separate data extraction functionality provides the ability to separately extract data from documents and add the extracted data to an index. In this case, between the stages of extraction and indexing, the extracted data can be serialized to an array of bytes and vice versa. This may be required, for example, for network transmission between search engine servers.

##### Public API changes

Class **ExtractedData** has been added to **GroupDocs.Search.Common** namespace.  
Method **GroupDocs.Search.Common.ExtractedData Deserialize(Byte[])** has been added to **GroupDocs.Search.Common.ExtractedData** class.  
Method **Byte[] Serialize()** has been added to **GroupDocs.Search.Common.ExtractedData** class.

Class **ExtractionOptions** has been added to **GroupDocs.Search.Options** namespace.  
Property **GroupDocs.Search.Common.IFieldExtractor CustomExtractor** has been added to **GroupDocs.Search.Options.ExtractionOptions** class.  
Property **System.String Encoding** has been added to **GroupDocs.Search.Options.ExtractionOptions** class.  
Property **GroupDocs.Search.Options.ImageIndexingOptions ImageIndexingOptions** has been added to **GroupDocs.Search.Options.ExtractionOptions** class.  
Property **GroupDocs.Search.Options.MetadataIndexingOptions MetadataIndexingOptions** has been added to **GroupDocs.Search.Options.ExtractionOptions** class.  
Property **GroupDocs.Search.Options.OcrIndexingOptions OcrIndexingOptions** has been added to **GroupDocs.Search.Options.ExtractionOptions** class.  
Property **Boolean UseRawTextExtraction** has been added to **GroupDocs.Search.Options.ExtractionOptions** class.

Class **ExtractorSettings** has been added to **GroupDocs.Search.Common** namespace.  
Property **GroupDocs.Search.Options.IndexType IndexType** has been added to **GroupDocs.Search.Common.ExtractorSettings** class.

Class **Extractor** has been added to **GroupDocs.Search** namespace.  
Event **System.EventHandler<GroupDocs.Search.Events.IndexErrorEventArgs> ErrorOccurred** has been added to **GroupDocs.Search.Extractor** class.  
Method **GroupDocs.Search.Common.ExtractedData Extract(GroupDocs.Search.Common.Document, GroupDocs.Search.Options.ExtractionOptions)** has been added to **GroupDocs.Search.Extractor** class.  
Event **System.EventHandler<GroupDocs.Search.Events.ImagePreparingEventArgs> ImagePreparing** has been added to **GroupDocs.Search.Extractor** class.  
Event **System.EventHandler<GroupDocs.Search.Events.PasswordRequiredEventArgs> PasswordRequired** has been added to **GroupDocs.Search.Extractor** class.  
Property **GroupDocs.Search.Common.ExtractorSettings Settings** has been added to **GroupDocs.Search.Extractor** class.

Property **System.String[] InnerPathParts** has been added to **GroupDocs.Search.Results.DocumentInfo** class.

Field **GroupDocs.Search.Common.DocumentSourceKind ExtractedData** has been added to **GroupDocs.Search.Common.DocumentSourceKind** enum.

Property **Int32 SegmentCount** has been added to **GroupDocs.Search.Common.IndexInfo** class.  
Property **Int32 TermCount** has been added to **GroupDocs.Search.Common.IndexInfo** class.

Method **Void Add(GroupDocs.Search.Common.ExtractedData[], GroupDocs.Search.Options.IndexingOptions)** has been added to **GroupDocs.Search.Index** class.

##### Use cases

The following example demonstrates how to perform separate data extraction an indexing.

```csharp
string indexFolder = @"c:\MyIndex";
string documentPath = @"c:\MyDocuments\MyDocument.pdf";

// Extracting data from a document
Extractor extractor = new Extractor();
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

// Indexing the data
ExtractedData[] data = new ExtractedData[]
{
    deserializedData
};
index.Add(data, new IndexingOptions());

// Searching in the index
SearchResult result = index.Search("Einstein");
```

### Implement support for .NET 6.0

This enhancement implements the ability to use the library and API on the .NET 6.0 platform.

##### Public API changes

None

##### Use cases

None

### Implement increase in indexing speed

This enhancement improves the indexing speed of the full-text search engine. The indexing speed of the first 1000 text documents has been increased by 30%. Significantly reduced the effect of indexing speed degradation with increasing index.

##### Public API changes

None

##### Use cases

None

### Fix unsupported document format errors

This fix eliminates indexing errors for some documents indexed from a file.

##### Public API changes

None

##### Use cases

None
