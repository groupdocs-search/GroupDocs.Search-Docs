---
id: custom-text-extractors
url: search/nodejs-java/custom-text-extractors
title: Custom text extractors
weight: 3
description: "GroupDocs.Search for Node.js supports indexing of many document formats. But there is also the possibility to implement support for any format other than the existing ones."
keywords: indexing of many document formats
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
GroupDocs.Search supports indexing of many document formats. But there is also the possibility to implement support for any format other than the existing ones.

The following example demonstrates how to implement a custom text extractor and use it for indexing.

```javascript
const indexFolder = 'c:/MyIndex/'; // Specify path to the index folder
const documentsFolder = 'c:/MyDocuments/'; // Specify path to a folder containing documents to search

const settings = new groupdocs.search.IndexSettings();
const logExtractor = java.newProxy('com.groupdocs.search.common.IFieldExtractor', {
  getExtensions: function () {
    const array = java.newArray('java.lang.String', ['.log']);
    return array;
  },
  getFields: function (filePath) {
    const fileName = path.resolve(filePath);
    const contents = fs.readFileSync(filePath, 'utf8');
    const fields = java.newArray('com.groupdocs.search.common.DocumentField', [
      new groupdocs.search.DocumentField('FileName', fileName),
      new groupdocs.search.DocumentField('Content', contents),
    ]);
    return fields;
  },
});
settings.getCustomExtractors().addItem(logExtractor); // Adding custom text extractor to the index settings

const index = new groupdocs.search.Index(indexFolder, settings, true); // Creating or loading an index

index.add(documentsFolder); // Indexing documents from the specified folder

const query1 = 'objection';
const result1 = index.search(query1); // Searching

const query2 = 'log';
const result2 = index.search(query2); // Searching
```

Note that custom extractors are not saved in an index and must be created and added each time the index is created or loaded. However, the same code can be used to create a new index and open an existing one. In this case, when opening an existing index, custom extractors from the index settings passed to the constructor will be used, the remaining index settings will be loaded from disk.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
