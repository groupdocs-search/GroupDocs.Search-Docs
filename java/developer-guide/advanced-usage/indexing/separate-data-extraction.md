---
id: separate-data-extraction
url: search/java/separate-data-extraction
title: Separate data extraction
weight: 19.5
description: "This article explains how to separately extract data from documents and add the extracted data to the index."
keywords: separate data extraction
productName: GroupDocs.Search for Java
hideChildren: False
---
The feature described in this article makes it possible to separate the operations of extracting data from a document and adding the extracted data to the index. The extracted data can be easily serialized and deserialized as needed.

This feature can be useful because the operation of extracting text and other data from documents can be very long for some formats. At the same time, adding data to the index is a rather lightweight operation. And thus, separating these operations can lead to a significant increase in the total time when the index is not busy performing any operations. And the subsequent addition of new data extractors will significantly increase the performance of indexing. In this case, data extractors can even work on separate servers, completely freeing the indexing server from extracting operations.

The [Extractor](https://reference.groupdocs.com/search/java/com.groupdocs.search/Extractor) class is used to extract data from documents. Upon completion of the operation, the extract method returns an instance of the [ExtractedData](https://reference.groupdocs.com/search/java/com.groupdocs.search.common/ExtractedData) class, which is used directly to add to the index.

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

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
