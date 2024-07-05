---
id: output-adapters
url: search/nodejs-java/output-adapters
title: Output adapters
weight: 13
description: "This article gives the knowledge about output adapters which are used to output generated HTML or plain text to various output objects."
keywords: Output adapters
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Output adapters are used to output generated HTML or plain text to various output objects. The following output adapters are currently supported:

*   [FileOutputAdapter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/FileOutputAdapter) is an output adapter that is used to output text to a file. The path to a file is specified in the constructor of the adapter.
*   [StreamOutputAdapter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/StreamOutputAdapter) is an output adapter that is used to output text to a stream. The stream is specified in the constructor of the adapter.
*   [StringOutputAdapter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/StringOutputAdapter) is an output adapter that is used to output text to a string. The resulting string can be accessed through the [getResult](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/StringOutputAdapter#getResult()) method of the adapter class.
*   [StructureOutputAdapter](https://reference.groupdocs.com/search/nodejs-java/groupdocs.search.common/StructureOutputAdapter) is an output adapter that is used to output the text of each field of the document separately. The resulting array of fields can be accessed through the [getResult](https://reference.groupdocs.com/search/nodejs-java/groupdocs.search.common/StructureOutputAdapter#getResult()) method of the adapter class.

The example below demonstates how to use adapters of different types.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index settings instance
const settings = new groupdocs.search.IndexSettings();
settings.setTextStorageSettings(new groupdocs.search.TextStorageSettings(groupdocs.search.Compression.High)); // Enabling the storage of extracted text in the index

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder, settings);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Getting list of indexed documents
const documents = index.getIndexedDocuments();

// Getting a document text
if (documents.length > 0) {
  const document = documents[0];

  // Output to a file
  const fileOutputAdapter = new groupdocs.search.FileOutputAdapter(groupdocs.search.OutputFormat.Html, 'c:/Result.html/');
  index.getDocumentText(document, fileOutputAdapter);

  // Output to a stream
  const stream = java.newInstanceSync('java.io.ByteArrayOutputStream');
  const streamOutputAdapter = new groupdocs.search.StreamOutputAdapter(groupdocs.search.OutputFormat.Html, stream);
  index.getDocumentText(document, streamOutputAdapter);

  // Output to a string
  const stringOutputAdapter = new groupdocs.search.StringOutputAdapter(groupdocs.search.OutputFormat.Html);
  index.getDocumentText(document, stringOutputAdapter);
  const result = stringOutputAdapter.getResult();
  //console.log(result);

  // Output to a structure
  const structureOutputAdapter = new groupdocs.search.StructureOutputAdapter(groupdocs.search.OutputFormat.PlainText);
  index.getDocumentText(document, structureOutputAdapter);
  const fields = structureOutputAdapter.getResult();
  console.log(document.toString());
  for (const field of fields) {
    console.log('\t' + field.getName());
  }
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
