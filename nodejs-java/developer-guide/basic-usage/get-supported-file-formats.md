---
id: get-supported-file-formats
url: search/nodejs-java/get-supported-file-formats
title: Get supported file formats
weight: 5
description: "This page describes how the search api is used to obtain a list of supported file types."
keywords: search api
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
The [getSupportedFileTypes](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.results/FileType#getSupportedFileTypes()) method of the [FileType](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.results/FileType) class is used to obtain a list of supported file types.

An example of obtaining a list of supported file types is presented below.

```javascript
const supportedFileTypes = java.callStaticMethodSync(
  'com.groupdocs.search.results.FileType',
  'getSupportedFileTypes',
);
const iterator = supportedFileTypes.iterator();
while (iterator.hasNext()) {
  const fileType = iterator.next();
  console.log(fileType.getExtension() + ' - ' + fileType.getDescription());
}
```

## More resources

### Advanced usage topics

To learn more about search features and get familiar how to enhance your search solution, please refer to the [advanced usage section]({{< ref "search/nodejs-java/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
