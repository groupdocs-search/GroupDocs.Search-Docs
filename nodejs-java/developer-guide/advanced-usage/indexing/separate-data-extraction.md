---
id: separate-data-extraction
url: search/nodejs-java/separate-data-extraction
title: Separate data extraction
weight: 19.5
description: "This article explains how to separately extract data from documents and add the extracted data to the index."
keywords: separate data extraction
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
The feature described in this article makes it possible to separate the operations of extracting data from a document and adding the extracted data to the index. The extracted data can be easily serialized and deserialized as needed.

This feature can be useful because the operation of extracting text and other data from documents can be very long for some formats. At the same time, adding data to the index is a rather lightweight operation. And thus, separating these operations can lead to a significant increase in the total time when the index is not busy performing any operations. And the subsequent addition of new data extractors will significantly increase the performance of indexing. In this case, data extractors can even work on separate servers, completely freeing the indexing server from extracting operations.

The [Extractor](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Extractor) class is used to extract data from documents. Upon completion of the operation, the extract method returns an instance of the [ExtractedData](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/ExtractedData) class, which is used directly to add to the index.

The following example demonstrates how to perform separate data extraction and indexing.

```javascript
const indexFolder = 'c:/MyIndex';
const documentPath = 'c:/MyDocuments/MyDocument.pdf';

// Extracting data from a document
const extractor = new groupdocs.search.Extractor();
const document = groupdocs.search.Document.createFromFile(documentPath);
const extractionOptions = new groupdocs.search.ExtractionOptions();
extractionOptions.setUseRawTextExtraction(false);
const extractedData = extractor.extract(document, extractionOptions);

// Serializing the data
const array1 = extractedData.serialize();
const array2 = [];
for (let i = 0; i < array1.length; i++) {
  array2[i] = array1[i];
}

// Deserializing the data
const buffer = java.newArray('byte', array2);
const deserializedData = groupdocs.search.ExtractedData.deserialize(buffer);

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Indexing the data
const data = java.newArray('com.groupdocs.search.ExtractedData', [deserializedData]);
index.add(data, new groupdocs.search.IndexingOptions());

// Searching in the index
const query = 'Einstein';
const result = index.search(query);
```

Note that when indexed documents change and need to be updated in the index, the same code must be executed. That is, data must be extracted separately from the updated documents and then the extracted data must be added to the index.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
