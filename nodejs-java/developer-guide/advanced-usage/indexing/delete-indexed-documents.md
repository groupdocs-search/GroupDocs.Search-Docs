---
id: delete-indexed-documents
url: search/nodejs-java/delete-indexed-documents
title: Delete indexed documents
weight: 4
description: "GroupDocs.Search for Node.js has the ability to remove individual documents from the index that are indexed from a stream or structure."
keywords: remove individual documents, remove individual documents from the index
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
GroupDocs.Search has the ability to remove individual documents from the index that are indexed from a stream or structure. To remove documents indexed by a file system path, see [Deleting indexed paths]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/delete-indexed-paths.md" >}}).

The following example shows how to remove documents from an index by document key.

```javascript
const filePath = 'c:/MyDocuments/SomeDocument.pdf';
const indexFolder = 'c:/MyIndex/';
const query = 'Einstein';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder, true);

// Indexing the document from stream
const documentKey = path.basename(filePath);
const documentLoader = java.newProxy('com.groupdocs.search.common.IDocumentLoader', {
  closeDocument: function () {},
  loadDocument: function () {
    const millis = java.callStaticMethodSync('java.lang.System', 'currentTimeMillis');
    const time = java.newInstanceSync('java.util.Date', millis);
    const extension = path.extname(filePath);
    const buffer = Utils.readBinaryFile(filePath);
    const array = Array.prototype.slice.call(buffer, 0);
    const buf = java.newArray('byte', array);
    const stream = java.newInstanceSync('java.io.ByteArrayInputStream', buf);
    const document = java.callStaticMethodSync(
      'com.groupdocs.search.Document',
      'createFromStream',
      documentKey,
      time,
      extension,
      stream,
    );
    return document;
  },
});

const document = java.callStaticMethodSync(
  'com.groupdocs.search.Document',
  'createLazy',
  groupdocs.search.DocumentSourceKind.Stream,
  documentKey,
  documentLoader,
);
const documents = java.newArray('com.groupdocs.search.Document', [document]);

// Subscribe to index events
const eventHandler = java.newProxy('com.groupdocs.search.events.EventHandler', {
  invoke: function (sender, args) {
    console.log(args.getMessage()); // Writing error messages to the console
  },
});
index.getEvents().ErrorOccurred.add(eventHandler);

index.add(documents, new groupdocs.search.IndexingOptions());

// Getting indexed documents from the index
const indexedDocuments1 = index.getIndexedDocuments();

// Writing indexed documents to the console
console.log('Indexed documents (' + indexedDocuments1.length + '):');
for (let i = 0; i < indexedDocuments1.length; i++) {
  const info = indexedDocuments1[i];
  console.log('\t' + info);
}

// Searching in the index
const searchResult1 = index.search(query);

// Deleting indexed document from the index
const documentKeys = java.newArray('java.lang.String', [documentKey]);
const deleteResult = index.delete(new groupdocs.search.UpdateOptions(), documentKeys);
console.log('\nDeleted documents: ' + deleteResult.getSuccessCount());

// Getting indexed paths after deletion
const indexedDocuments2 = index.getIndexedDocuments();

console.log('\nIndexed documents (' + indexedDocuments2.length + '):');
for (let i = 0; i < indexedDocuments2.length; i++) {
  const info = indexedDocuments2[i];
  console.log('\t' + info);
}

// Searching in the index
const searchResult2 = index.search(query);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
