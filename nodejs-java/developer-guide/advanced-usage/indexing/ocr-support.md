---
id: ocr-support
url: search/nodejs-java/ocr-support
title: OCR support
weight: 18
description: ""
keywords: 
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
OCR support means the ability to connect an external module (library) for the recognition of printed text (optical character recognition, OCR) on images, either separate or embedded in documents.

To connect OCR, you need to implement the IOcrConnector interface in the client code.

The following example demonstrates how to implement the OCR connector using com.aspose.ocr library for text recognition in images.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';
const query = 'Einstein';

// Creating an index
const index = new groupdocs.search.Index(indexFolder, true);

// Subscribing to the ErrorOccurred event
index.getEvents().ErrorOccurred.add(
  java.newProxy('com.groupdocs.search.events.EventHandler', {
    invoke: function (sender, args) {
      console.log(args.getMessage());
    },
  }),
);

// Setting the OCR indexing options
const options = new groupdocs.search.IndexingOptions();
options.getOcrIndexingOptions().setEnabledForSeparateImages(true);
options.getOcrIndexingOptions().setEnabledForEmbeddedImages(true);
const ocrConnector = java.newProxy('com.groupdocs.search.options.IOcrConnector', {
  recognize: function (context) {
    switch (String(context.getImageLocation())) {
      case 'Separate':
      case 'Embedded':
      case 'ContainerItem':
        const image = java.callStaticMethodSync('javax.imageio.ImageIO', 'read', context.getImageStream());
        const asposeOcr = new groupdocs.search.AsposeOcr();
        const result = asposeOcr.RecognizePage(image);
        return result;
      default:
        throw new Error('The image type is not supported: ' + context.getImageLocation());
    }
  },
});
options.getOcrIndexingOptions().setOcrConnector(ocrConnector);

// Indexing documents in a document folder
index.add(documentsFolder, options);

// Searching in the index
const result = index.search(query);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

* [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

* [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
