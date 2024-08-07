---
id: optimize-index
url: search/nodejs-java/optimize-index
title: Optimize index
weight: 19
description: "This article explains that how to optimize index to reduce the number of segments in an index using Java."
keywords: optimize index
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
An index optimization operation allows you to reduce the number of segments in an index, thereby increasing search performance in the index.

To perform this operation, there is an overload that takes the [MergeOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/MergeOptions) instance as a parameter. In the [MergeOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/MergeOptions) class, there is a method for specifying a cancellation object and a method for specifying whether to perform the operation asynchronously. By default, the operation is performed synchronously.

The following example demonstrates how to perform the index optimization.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder1 = 'c:/MyDocuments1/';
const documentsFolder2 = 'c:/MyDocuments2/';
const documentsFolder3 = 'c:/MyDocuments3/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder, true);

index.add(documentsFolder1); // Indexing documents from the specified folder
index.add(documentsFolder2); // Each call to Add creates at least one new segment in the index
index.add(documentsFolder3);

const options = new groupdocs.search.MergeOptions();
options.setCancellation(new groupdocs.search.Cancellation()); // Creating cancellation object to be able to cancel the operation
options.getCancellation().cancelAfter(30000); // Setting maximum duration of the operation to 30 seconds

// Merging segments of the index
index.optimize(options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
