---
id: indexing-from-different-sources
url: search/nodejs-java/indexing-from-different-sources
title: Indexing from different sources
weight: 10
description: "GroupDocs.Search allows indexing documents from various sources."
keywords: indexing documents from various sources, indexing documents
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
GroupDocs.Search allows indexing documents from various sources:

- From files in the file system.
- From a stream.
- From a data structure as an array of fields.

The library also allows indexing from all presented sources with lazy initialization.

Please note that the update operation automatically generates a list of changed files only when indexing from the local file system. When indexing from streams or structures, documents cannot be updated with the update operation. To update documents from these sources, you must re-index the modified documents by passing their keys and updated data to the [add](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#add(com.groupdocs.search.Document[],%20com.groupdocs.search.options.IndexingOptions)) method.

## Indexing from a file

It should be borne in mind that the [add](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#add(com.groupdocs.search.Document[],%20com.groupdocs.search.options.IndexingOptions)) method with the parameter of type [Document](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Document)[] allows indexing only documents individually, and not entire folders. The advantage of using this method overload is that you can add attributes and additional fields to the indexed document before calling the [add](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#add(com.groupdocs.search.Document[],%20com.groupdocs.search.options.IndexingOptions)) method. The following example demonstrates how to index a document from a file.

```javascript
const indexFolder = 'c:/MyIndex';
const documentFilePath = 'c:/MyDocuments/ExampleDocument.pdf';

// Creating an index
const settings = new groupdocs.search.IndexSettings();
const index = new groupdocs.search.Index(indexFolder, settings);

// Creating a document object
const document = groupdocs.search.Document.createFromFile(documentFilePath);
const documents = java.newArray('com.groupdocs.search.Document', [document]);

// Indexing document from the file
const options = new groupdocs.search.IndexingOptions();
options.setUseRawTextExtraction(false);
index.add(documents, options);
```

## Indexing from a stream

The following example demonstrates how to index a document from a stream.

```javascript
const indexFolder = 'c:/MyIndex';
const documentFilePath = 'c:/MyDocuments/ExampleDocument.pdf';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Creating a document object
const stream = java.newInstanceSync('java.io.FileInputStream', documentFilePath); // Opening a stream
const document = groupdocs.search.Document.createFromStream(documentFilePath, new Date(), '.pdf', stream);
const documents = java.newArray('com.groupdocs.search.Document', [document]);

// Indexing document from the stream
const options = new groupdocs.search.IndexingOptions();
index.add(documents, options);

// Closing the document stream after indexing is complete
stream.close();
```

## Indexing from a structure

The following example demonstrates how to index a document from a structure.

```javascript
const indexFolder = 'c:/MyIndex';
const documentFilePath = 'c:/MyDocuments/ExampleDocument.pdf';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Creating a document object
const text = fs.readFileSync(documentFilePath, 'utf8').toString();
const fields = java.newArray('com.groupdocs.search.common.DocumentField', [
  new groupdocs.search.DocumentField(groupdocs.search.CommonFieldNames.Content, text),
]);
const document = groupdocs.search.Document.createFromStructure(
  'ExampleDocument',
  java.newInstanceSync('java.util.Date'),
  fields,
);
const documents = java.newArray('com.groupdocs.search.Document', [document]);

// Indexing document from the structure
const options = new groupdocs.search.IndexingOptions();
index.add(documents, options);
```

## Indexing from URL

The following example demonstrates how to index a document by URL when lazy initialized.

```javascript
const indexFolder = 'c:/MyIndex';
const url = 'http://example.com/ExampleDocument.pdf';

// Creating an index
const settings = new groupdocs.search.IndexSettings();
settings.setTextStorageSettings(new groupdocs.search.TextStorageSettings(groupdocs.search.Compression.High));
const index = new groupdocs.search.Index(indexFolder, settings, true);

index.getEvents().ErrorOccurred.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      console.log(args.getMessage());
    },
  }),
);

// Creating a document object
const documentKey = url;
const extension = path.extname(url);
const documentLoader = java.newProxy('com.groupdocs.search.common.IDocumentLoader', {
  loadDocument: function () {
    const urlInstance = java.newInstanceSync('java.net.URL', url);
    const stream = urlInstance.openStream();
    const document = groupdocs.search.Document.createFromStream(
      documentKey,
      java.newInstanceSync('java.util.Date'),
      extension,
      stream,
    );
    return document;
  },
  closeDocument: function () {},
});
const document = groupdocs.search.Document.createLazy(
  groupdocs.search.DocumentSourceKind.Stream,
  documentKey,
  documentLoader,
);
const documents = java.newArray('com.groupdocs.search.Document', [document]);

// Indexing the lazy-loaded document
const options = new groupdocs.search.IndexingOptions();
options.setUseRawTextExtraction(false);
index.add(documents, options);
```

## Indexing from FTP

The following example demonstrates how to index a document from FTP when lazy initialized.

```javascript
const indexFolder = 'c:/MyIndex';
const url = 'ftp://example.com/ExampleDocument.pdf';

// Creating an index
const settings = new groupdocs.search.IndexSettings();
settings.setTextStorageSettings(new groupdocs.search.TextStorageSettings(groupdocs.search.Compression.High));
const index = new groupdocs.search.Index(indexFolder, settings, true);

index.getEvents().ErrorOccurred.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      console.log(args.getMessage());
    },
  }),
);

// Creating a document object
const documentKey = url;
const extension = path.extname(url);
const documentLoader = java.newProxy('com.groupdocs.search.common.IDocumentLoader', {
  loadDocument: function () {
    const urlInstance = java.newInstanceSync('java.net.URL', url);
    const stream = urlInstance.openStream();
    const document = groupdocs.search.Document.createFromStream(
      documentKey,
      java.newInstanceSync('java.util.Date'),
      extension,
      stream,
    );
    return document;
  },
  closeDocument: function () {},
});

const document = groupdocs.search.Document.createLazy(
  groupdocs.search.DocumentSourceKind.Stream,
  documentKey,
  documentLoader,
);
const documents = java.newArray('com.groupdocs.search.Document', [document]);

// Indexing the lazy-loaded document
const options = new groupdocs.search.IndexingOptions();
options.setUseRawTextExtraction(false);
index.add(documents, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
