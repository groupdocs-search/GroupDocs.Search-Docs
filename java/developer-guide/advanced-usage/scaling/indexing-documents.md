---
id: indexing-documents
url: search/java/indexing-documents
title: Indexing documents
weight: 40
description: "This page contains information about indexing documents in the search network."
keywords: search network indexing, distributed index indexing, adding documents in search network
productName: GroupDocs.Search for Java
hideChildren: False
---
Documents can be added to the search network using the [add](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/#add-com.groupdocs.search.Document---java.lang.String---com.groupdocs.search.options.IndexingOptions-) method of the [Indexer](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/) class.

The first parameter of the [add](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/#add-com.groupdocs.search.Document---java.lang.String---com.groupdocs.search.options.IndexingOptions-) method is an array of documents to be added. Currently, documents created from a stream or from a structure are supported.

The second parameter of the [add](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/#add-com.groupdocs.search.Document---java.lang.String---com.groupdocs.search.options.IndexingOptions-) method is an array of optional passwords for opening added documents.

The third parameter of the [add](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/#add-com.groupdocs.search.Document---java.lang.String---com.groupdocs.search.options.IndexingOptions-) method is the indexing options object.

The following code example demonstrates adding documents from a stream.

```java
InputStream[] streams = new InputStream[filePaths.length];
Document[] documents = new Document[filePaths.length];
String[] passwords = new String[filePaths.length];
for (int i = 0; i < filePaths.length; i++) {
    String filePath = filePaths[i];
    String extension = FilenameUtils.getExtension(filePath);
    InputStream stream = new FileInputStream(filePath);
    streams[i] = stream;
    Document document = Document.createFromStream(
        filePath,
        new Date(),
        extension,
        stream);
    documents[i] = document;
}

Indexer indexer = node.getIndexer();
IndexingOptions options = new IndexingOptions();
options.setUseRawTextExtraction(false);
options.getImageIndexingOptions().setEnabledForSeparateImages(true);
options.getImageIndexingOptions().setEnabledForEmbeddedImages(true);
options.getImageIndexingOptions().setEnabledForContainerItemImages(true);
options.getOcrIndexingOptions().setEnabledForSeparateImages(true);
options.getOcrIndexingOptions().setEnabledForEmbeddedImages(true);
options.getOcrIndexingOptions().setEnabledForContainerItemImages(true);
indexer.add(documents, passwords, options);

for (InputStream stream : streams) {
    stream.close();
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
