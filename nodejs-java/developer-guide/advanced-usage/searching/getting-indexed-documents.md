---
id: getting-indexed-documents
url: search/nodejs-java/getting-indexed-documents
title: Getting indexed documents
weight: 7
description:  "This article explains how to get a list of indexed documents from an index, and how to get the text of indexed documents in HTML or plain text format."
keywords: Getting indexed documents
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page contains a description of how to get a list of indexed documents from an index, and how to get the text of indexed documents in HTML or plain text format.

## Getting indexed documents

To get a list of indexed documents from an index, use the [getIndexedDocuments](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#getIndexedDocuments()) method of the [Index](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index) class. Documents with the extensions ZIP, PST, OST can also contain internal documents. To get a list of internal documents, use the [getIndexedDocumentItems](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#getIndexedDocumentItems(com.groupdocs.search.results.DocumentInfo)) method of the [Index](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index) class. For ZIP archives, this way you can access documents of arbitrary nesting depth. An example of obtaining a list of documents from an index is presented below.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Getting list of indexed documents
const documents = index.getIndexedDocuments();
for (const document of documents) {
  console.log(document.getFilePath());
  const items = index.getIndexedDocumentItems(document); // Getting list of document items
  for (const item of items) {
    console.log('\t' + item.getInnerPath());
  }
}
```

## Getting text of indexed documents

The text of the indexed document can also be extracted from an index if the option to save the text of documents in the index has been enabled. If this option was not enabled when creating an index, then when the [getDocumentText](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#getDocumentText(com.groupdocs.search.results.DocumentInfo,%20com.groupdocs.search.common.OutputAdapter)) method of the [Index](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index) class is called, the text of the document will be retrieved again. Details about saving the text of documents in an index can be found on the page [Storing text of indexed documents]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}).

The generated text of the document is passed to an instance of a class derived from the abstract class [OutputAdapter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/OutputAdapter). Details on the output adapters are presented on the page [Output adapters]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/output-adapters.md" >}}).

After generating the text of a document into a file, this file can be opened by an Internet browser. The following example shows how to extract document text from an index.

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
  const outputAdapter = new groupdocs.search.FileOutputAdapter(
      groupdocs.search.OutputFormat.Html,
    Utils.OutputPath + 'AdvancedUsage/Searching/gettingIndexedDocuments/Text.html',
  );
  index.getDocumentText(documents[0], outputAdapter);

  // Getting list of files in the archive
  const items = index.getIndexedDocumentItems(documents[0]);
  if (items.length > 0) {
    const outputAdapter2 = new groupdocs.search.FileOutputAdapter(
      groupdocs.search.OutputFormat.Html,
      Utils.OutputPath + 'AdvancedUsage/Searching/gettingIndexedDocuments/ItemText.html',
    );
    index.getDocumentText(items[0], outputAdapter2);
  }
}
```

To extract the text of a document from an index, the method overloading is also presented, which takes an instance of the [TextOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions) class as a parameter. In this class, the following options can be specified:

*   [setCustomExtractor](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions#setCustomExtractor(com.groupdocs.search.common.IFieldExtractor)) method sets an extractor used during indexing, it is necessary if the text of the document was not saved in the index;
*   [setAdditionalFields](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions#setAdditionalFields(com.groupdocs.search.common.DocumentField%5B%5D)) method sets additional document fields added during document indexing which are also necessary if the document text was not saved in the index;
*   [setCancellation](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions#setCancellation(com.groupdocs.search.common.Cancellation)) method sets an object used to cancel the operation;
*   [getMetadataIndexingOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions#getMetadataIndexingOptions()) method returns an object for specifying metadata indexing options.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
