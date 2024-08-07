---
id: merge-indexes
url: search/nodejs-java/merge-indexes
title: Merge indexes
weight: 17
description: ""
keywords: 
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
The merge operation is designed to combine two or more indexes into one index to accelerate the search and to simplify the work with indexes. When merging, only the index at which the [merge](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#merge(com.groupdocs.search.Index,%20com.groupdocs.search.options.MergeOptions)) method was called is changed. This index as a result of the operation contains all the documents that were contained in all indexes together. The second index or index repository after the merge can be deleted to free up disk space.

The [MergeOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/MergeOptions) class contains a method for specifying a cancellation object, as well as a method for specifying whether the operation will be performed asynchronously. By default, index merging operation is performed synchronously.

The [Index](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index) class has method overloads for merging a single index and a whole index repository. Before performing a merge, checks are made to ensure that the indexes are of the same type. Merging is performed only on those indexes that have the same [IndexType](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/IndexType) value as the index on which the [merge](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#merge(com.groupdocs.search.Index,%20com.groupdocs.search.options.MergeOptions)) method is called.

The following example demonstrates the merging of two indexes.

```javascript
const indexFolder1 = 'c:/MyIndex1/';
const indexFolder2 = 'c:/MyIndex2/';
const documentsFolder1 = 'c:/MyDocuments1/';
const documentsFolder2 = 'c:/MyDocuments2/';

const index1 = new groupdocs.search.Index(indexFolder1); // Creating index1
index1.add(documentsFolder1); // Indexing documents

const index2 = new groupdocs.search.Index(indexFolder2); // Creating index2
index2.add(documentsFolder2); // Indexing documents

const options = new groupdocs.search.MergeOptions();
options.setCancellation(new groupdocs.search.Cancellation()); // Creating cancellation object to be able to cancel the oparation
options.getCancellation().cancelAfter(5000); // Setting a time limit for the operation of 5 seconds

// Merging index2 into index1
// Files of index2 will not be changed
index1.merge(index2, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
