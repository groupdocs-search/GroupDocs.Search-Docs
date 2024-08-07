---
id: storing-text-of-indexed-documents
url: search/nodejs-java/storing-text-of-indexed-documents
title: Storing text of indexed documents
weight: 20
description: "This article explains that how to store text of indexed documents using Java."
keywords: store text of indexed documents, Text extracted from indexed documents
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Text extracted from indexed documents can be stored in an index to provide the extracted text to the user faster when called the [getDocumentText](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#getDocumentText(com.groupdocs.search.results.DocumentInfo,%20com.groupdocs.search.common.OutputAdapter)) method, as well as to accelerate text generation with highlighting of search results.

To specify storage parameters, use the [setTextStorageSettings](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setTextStorageSettings(com.groupdocs.search.options.TextStorageSettings)) method of the [IndexSettings](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings) class. The default value is null, which means that the text of the documents is not stored in the index.

When saving text in the index, the values defined in a [Compression](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/Compression) class are used to specify the compression ratio of the saved text. Compression can be normal, high, or text can be saved without compression. The choice of compression ratio affects the final size of the index, as well as the speed of indexing. A high compression ratio reduces index size and indexing speed, and the lack of compression makes index size and indexing speed maximum. The default compression ratio is normal.

The example below demonstrates storing text in an index using the high compression ratio.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index settings instance
const settings = new groupdocs.search.IndexSettings();
settings.setTextStorageSettings(new groupdocs.search.TextStorageSettings(groupdocs.search.Compression.High)); // Setting high compression ratio for the index text storage

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder, settings, true);

// Indexing documents
index.add(documentsFolder);

// Now the index contains the text of all indexed documents,
// so the operations of getting the text of documents and highlighting occurrences are faster.

// Searching
const query = 'Einstein';
const result = index.search(query);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
