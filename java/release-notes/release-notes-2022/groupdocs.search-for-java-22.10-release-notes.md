---
id: groupdocs-search-for-java-22-10-release-notes
url: search/java/groupdocs-search-for-java-22-10-release-notes
title: com.groupdocs.search for Java 22.10 Release Notes
weight: 10
description: ""
keywords: 
productName: com.groupdocs.search for Java
hideChildren: False
---

{{< alert style="info" >}}This page contains release notes for com.groupdocs.search for Java 22.10{{< /alert >}}

## Major Features

There are the following features, enhancements, and fixes in this release:

- Implement reverse image search functionality
- Implement separate data extraction and indexing
- Implement increase in indexing speed

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2666 | Implement reverse image search functionality | Feature |
| SEARCHNET-2668 | Implement separate data extraction and indexing | Feature |
| SEARCHNET-2690 | Implement increase in indexing speed | Enhancement |

## Public API and Backward Incompatible Changes

### Implement reverse image search functionality

The reverse image search functionality provides the ability to search for similar images in archives, documents, and individual files. For more information, see the [Reverse image search](https://docs.groupdocs.com/search/java/reverse-image-search/) documentation page.

##### Public API changes

Class **DocumentImage** has been added to **com.groupdocs.search.common** package.  
Method **com.groupdocs.search.common.DocumentField[] getFields()** has been added to **com.groupdocs.search.common.DocumentImage** class.  
Method **com.groupdocs.search.common.ImageFrame[] getFrames()** has been added to **com.groupdocs.search.common.DocumentImage** class.  
Method **int getImageIndex()** has been added to **com.groupdocs.search.common.DocumentImage** class.

Class **FoundImageFrame** has been added to **com.groupdocs.search.results** package.  
Method **com.groupdocs.search.results.DocumentInfo getDocumentInfo()** has been added to **com.groupdocs.search.results.FoundImageFrame** class.  
Method **int getFrameIndex()** has been added to **com.groupdocs.search.results.FoundImageFrame** class.  
Method **int getHashDifferences()** has been added to **com.groupdocs.search.results.FoundImageFrame** class.  
Method **int getImageIndex()** has been added to **com.groupdocs.search.results.FoundImageFrame** class.

Class **ImageFrame** has been added to **com.groupdocs.search.common** package.  
Method **int getIndex()** has been added to **com.groupdocs.search.common.ImageFrame** class.

Class **ImageIndexingOptions** has been added to **com.groupdocs.search.options** package.  
Method **boolean getEnabledForContainerItemImages()** has been added to **com.groupdocs.search.options.ImageIndexingOptions** class.  
Method **void setEnabledForContainerItemImages(boolean)** has been added to **com.groupdocs.search.options.ImageIndexingOptions** class.  
Method **boolean getEnabledForEmbeddedImages()** has been added to **com.groupdocs.search.options.ImageIndexingOptions** class.  
Method **void setEnabledForEmbeddedImages(boolean)** has been added to **com.groupdocs.search.options.ImageIndexingOptions** class.  
Method **boolean getEnabledForSeparateImages()** has been added to **com.groupdocs.search.options.ImageIndexingOptions** class.
Method **void setEnabledForSeparateImages(boolean)** has been added to **com.groupdocs.search.options.ImageIndexingOptions** class.

Class **ImagePreparingEventArgs** has been added to **com.groupdocs.search.events** package.  
Method **String getDocumentKey()** has been added to **com.groupdocs.search.events.ImagePreparingEventArgs** class.  
Method **com.groupdocs.search.common.ImageFrame[] getImageFrames()** has been added to **com.groupdocs.search.events.ImagePreparingEventArgs** class.  
Method **int getImageIndex()** has been added to **com.groupdocs.search.events.ImagePreparingEventArgs** class.  
Method **java.io.InputStream getImageStream()** has been added to **com.groupdocs.search.events.ImagePreparingEventArgs** class.  
Method **String[] getInnerPath()** has been added to **com.groupdocs.search.events.ImagePreparingEventArgs** class.

Class **ImageSearchOptions** has been added to **com.groupdocs.search.options** package.  
Method **int getHashDifferences()** has been added to **com.groupdocs.search.options.ImageSearchOptions** class.  
Method **void setHashDifferences(int)** has been added to **com.groupdocs.search.options.ImageSearchOptions** class.  
Method **int getMaxResultCount()** has been added to **com.groupdocs.search.options.ImageSearchOptions** class.  
Method **void setMaxResultCount(int)** has been added to **com.groupdocs.search.options.ImageSearchOptions** class.  
Method **com.groupdocs.search.options.ISearchDocumentFilter getSearchDocumentFilter()** has been added to **com.groupdocs.search.options.ImageSearchOptions** class.
Method **void setSearchDocumentFilter(com.groupdocs.search.options.ISearchDocumentFilter)** has been added to **com.groupdocs.search.options.ImageSearchOptions** class.

Class **ImageSearchResult** has been added to **com.groupdocs.search.results** package.  
Method **com.groupdocs.search.results.FoundImageFrame getFoundImage(int)** has been added to **com.groupdocs.search.results.ImageSearchResult** class.  
Method **int getImageCount()** has been added to **com.groupdocs.search.results.ImageSearchResult** class.

Class **SearchImage** has been added to **com.groupdocs.search.common** package.  
Method **com.groupdocs.search.common.SearchImage create(String)** has been added to **com.groupdocs.search.common.SearchImage** class.  
Method **com.groupdocs.search.common.SearchImage create(String, int)** has been added to **com.groupdocs.search.common.SearchImage** class.  
Method **com.groupdocs.search.common.SearchImage create(java.io.InputStream)** has been added to **com.groupdocs.search.common.SearchImage** class.  
Method **com.groupdocs.search.common.SearchImage create(java.io.InputStream, int)** has been added to **com.groupdocs.search.common.SearchImage** class.

Method **com.groupdocs.search.options.ImageIndexingOptions getImageIndexingOptions()** has been added to **com.groupdocs.search.options.IndexingOptions** class.  
Method **com.groupdocs.search.options.ImageIndexingOptions getImageIndexingOptions()** has been added to **com.groupdocs.search.options.TextOptions** class.  
Method **com.groupdocs.search.options.ImageIndexingOptions getImageIndexingOptions()** has been added to **com.groupdocs.search.options.UpdateOptions** class.

Field **Event<EventHandler<ImagePreparingEventArgs>> ImagePreparing** has been added to **com.groupdocs.search.events.EventHub** class.

Method **boolean getEnabledForContainerItemImages()** has been added to **com.groupdocs.search.options.OcrIndexingOptions** class.
Method **void setEnabledForContainerItemImages(boolean value)** has been added to **com.groupdocs.search.options.OcrIndexingOptions** class.

Method **com.groupdocs.search.results.ImageSearchResult Search(com.groupdocs.search.common.SearchImage, com.groupdocs.search.options.ImageSearchOptions)** has been added to **com.groupdocs.search.Index** class.

##### Use cases

The following example demonstrates how to perform reverse image search in an index.

```java
String indexFolder = "c:\\MyIndex";
String documentsFolder = "c:\\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Setting the image indexing options
IndexingOptions indexingOptions = new IndexingOptions();
indexingOptions.getImageIndexingOptions().setEnabledForContainerItemImages(true);
indexingOptions.getImageIndexingOptions().setEnabledForEmbeddedImages(true);
indexingOptions.getImageIndexingOptions().setEnabledForSeparateImages(true);

// Indexing documents in a document folder
index.add(documentsFolder, indexingOptions);

// Setting the image search options
ImageSearchOptions imageSearchOptions = new ImageSearchOptions();
imageSearchOptions.setHashDifferences(10);
imageSearchOptions.setMaxResultCount(10000);
imageSearchOptions.setSearchDocumentFilter(SearchDocumentFilter.createFileExtension(".zip", ".png", ".jpg"));

// Creating a reference image for search
SearchImage searchImage = SearchImage.create("c:\\MyDocuments\\image.png");

// Searching in the index
ImageSearchResult result = index.search(searchImage, imageSearchOptions);

System.out.print("Images found: " + result.getImageCount());
for (int i = 0; i < result.getImageCount(); i++) {
    FoundImageFrame image = result.getFoundImage(i);
    System.out.print(image.getDocumentInfo().toString());
}
```

### Implement separate data extraction and indexing

The separate data extraction functionality provides the ability to separately extract data from documents and add the extracted data to an index. In this case, between the stages of extraction and indexing, the extracted data can be serialized to an array of bytes and vice versa. This may be required, for example, for network transmission between search engine servers. For more information, see the [Separate data extraction](https://docs.groupdocs.com/search/java/separate-data-extraction/) documentation page.

##### Public API changes

Class **ExtractedData** has been added to **com.groupdocs.search.common** namespace.  
Method **com.groupdocs.search.common.ExtractedData deserialize(byte[])** has been added to **com.groupdocs.search.common.ExtractedData** class.  
Method **byte[] serialize()** has been added to **com.groupdocs.search.common.ExtractedData** class.

Class **ExtractionOptions** has been added to **com.groupdocs.search.options** namespace.  
Method **com.groupdocs.search.common.IFieldExtractor getCustomExtractor()** has been added to **com.groupdocs.search.options.ExtractionOptions** class.  
Method **void setCustomExtractor(com.groupdocs.search.common.IFieldExtractor)** has been added to **com.groupdocs.search.options.ExtractionOptions** class.  
Method **String getEncoding()** has been added to **com.groupdocs.search.options.ExtractionOptions** class.  
Method **void setEncoding(String)** has been added to **com.groupdocs.search.options.ExtractionOptions** class.  
Method **com.groupdocs.search.options.ImageIndexingOptions getImageIndexingOptions()** has been added to **com.groupdocs.search.options.ExtractionOptions** class.  
Method **com.groupdocs.search.options.MetadataIndexingOptions getMetadataIndexingOptions()** has been added to **com.groupdocs.search.options.ExtractionOptions** class.  
Method **com.groupdocs.search.options.OcrIndexingOptions getOcrIndexingOptions()** has been added to **com.groupdocs.search.options.ExtractionOptions** class.  
Method **boolean getUseRawTextExtraction()** has been added to **com.groupdocs.search.options.ExtractionOptions** class.
Method **void setUseRawTextExtraction(boolean)** has been added to **com.groupdocs.search.options.ExtractionOptions** class.

Class **ExtractorSettings** has been added to **com.groupdocs.search.common** namespace.  
Method **com.groupdocs.search.options.IndexType getIndexType()** has been added to **com.groupdocs.search.common.ExtractorSettings** class.
Method **void setIndexType(com.groupdocs.search.options.IndexType)** has been added to **com.groupdocs.search.common.ExtractorSettings** class.

Class **Extractor** has been added to **com.groupdocs.search** namespace.  
Field **Event<EventHandler<IndexErrorEventArgs>> ErrorOccurred** has been added to **com.groupdocs.search.Extractor** class.  
Method **com.groupdocs.search.common.ExtractedData extract(com.groupdocs.search.common.Document, com.groupdocs.search.options.ExtractionOptions)** has been added to **com.groupdocs.search.Extractor** class.  
Field **Event<EventHandler<ImagePreparingEventArgs>> ImagePreparing** has been added to **com.groupdocs.search.Extractor** class.  
Field **Event<EventHandler<PasswordRequiredEventArgs>> PasswordRequired** has been added to **com.groupdocs.search.Extractor** class.  
Method **com.groupdocs.search.common.ExtractorSettings getSettings()** has been added to **com.groupdocs.search.Extractor** class.

Method **String[] getInnerPathParts()** has been added to **com.groupdocs.search.results.DocumentInfo** class.

Value **ExtractedData** has been added to **com.groupdocs.search.common.DocumentSourceKind** enum.

Method **int getSegmentCount()** has been added to **com.groupdocs.search.common.IndexInfo** class.  
Method **int getTermCount()** has been added to **com.groupdocs.search.common.IndexInfo** class.

Method **void add(com.groupdocs.search.common.ExtractedData[], com.groupdocs.search.options.IndexingOptions)** has been added to **com.groupdocs.search.Index** class.

##### Use cases

The following example demonstrates how to perform separate data extraction and indexing.

```java
String indexFolder = "c:\\MyIndex";
String documentPath = "c:\\MyDocuments\\MyDocument.pdf";

// Extracting data from a document
Extractor extractor = new Extractor();
Document document = Document.createFromFile(documentPath);
ExtractionOptions extractionOptions = new ExtractionOptions();
extractionOptions.setUseRawTextExtraction(false);
ExtractedData extractedData = extractor.extract(document, extractionOptions);

// Serializing the data
byte[] array = extractedData.serialize();

// Deserializing the data
ExtractedData deserializedData = ExtractedData.deserialize(array);

// Creating an index
com.groupdocs.search.Index index = new com.groupdocs.search.Index(indexFolder);

// Indexing the data
ExtractedData[] data = new ExtractedData[] {
    deserializedData
};
index.add(data, new IndexingOptions());

// Searching in the index
SearchResult result = index.search("Einstein");
```

### Implement increase in indexing speed

This enhancement improves the indexing speed of the full-text search engine. The indexing speed of the first 1000 text documents has been increased by 30%. Significantly reduced the effect of indexing speed degradation with increasing index.

##### Public API changes

None

##### Use cases

None
