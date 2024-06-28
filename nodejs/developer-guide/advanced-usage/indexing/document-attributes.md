---
id: document-attributes
url: search/nodejs-java/document-attributes
title: Document attributes
weight: 6
description: "Document attributes is a special feature designed for marking indexed documents with text labels without the need for re-indexing."
keywords: Document attributes, marking indexed documents, re-indexing 
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Document attributes is a special feature designed for marking indexed documents with text labels without the need for re-indexing. Added attributes can be further used to filter documents during the search.

To add and delete attributes of indexed documents, use the [changeAttributes](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#changeAttributes(com.groupdocs.search.AttributeChangeBatch)) method of the [Index](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index) class. This method accepts an [AttributeChangeBatch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/AttributeChangeBatch) object containing the required attribute changes as a parameter.

The following example demonstrates how to add and remove attributes from indexed documents.

```javascript
const indexFolder = 'c:/MyIndex';
const documentFolder = 'c:/MyDocuments';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents in a document folder
index.add(documentFolder);

const documents = index.getIndexedDocuments();

// Creating an attribute change container object
const batch = new groupdocs.search.AttributeChangeBatch();
// Adding one attribute to all indexed documents
batch.addToAll('public');
// Removing one attribute from one indexed document
batch.remove(documents[0].getFilePath(), 'public');
// Adding two attributes to one indexed document
batch.add(documents[0].getFilePath(), 'main', 'key');

// Applying attribute changes in the index
index.changeAttributes(batch);

// Searching in the index
const options = new groupdocs.search.SearchOptions();
// Creating a document filter by attribute
options.setSearchDocumentFilter(groupdocs.search.SearchDocumentFilter.createAttribute('main'));
const query = 'Einstein';
const result = index.search(query, options);
```

Attributes can be associated with documents during indexing using the [FileIndexing](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#FileIndexing) event. The following example demonstrates this.

```javascript
const indexFolder = 'c:/MyIndex';
const documentFolder = 'c:/MyDocuments';

// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Subscribing to the FileIndexing event to add attributes
index.getEvents().FileIndexing.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      if (args.getDocumentFullPath().endsWith('Lorem ipsum.pdf')) {
        // Adding two attributes
        args.setAttributes(['main', 'key']);
      }
    },
  }),
);

// Indexing documents in a document folder
index.add(documentFolder);

// Searching in the index
const options = new groupdocs.search.SearchOptions();
// Creating a document filter by attribute
options.setSearchDocumentFilter(groupdocs.search.SearchDocumentFilter.createAttribute('main'));
const query = 'Einstein';
const result = index.search(query, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
