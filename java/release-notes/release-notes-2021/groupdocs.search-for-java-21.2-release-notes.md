---
id: groupdocs-search-for-java-21-2-release-notes
url: search/java/groupdocs-search-for-java-21-2-release-notes
title: GroupDocs.Search for Java 21.2 Release Notes
weight: 30
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 21.2{{< /alert >}}

## Major Features

There are the following improvementes in this release:

  - Implement ability to perform boolean search with stop words
  - Implement ability to get status of indexed file
  - Implement ability to delete files indexed from stream

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2486 | Implement ability to perform boolean search with stop words | Improvement |
| SEARCHNET-2487 | Implement ability to get status of indexed file | Improvement |
| SEARCHNET-2485 | Implement ability to delete files indexed from stream | Improvement |

## Public API and Backward Incompatible Changes

### Implement ability to perform boolean search with stop words

This improvement allows to perform boolean search on queries containing stop words. Now stop words in a boolean search query do not lead to an empty result, but are simply ignored.

##### Public API changes

None.

##### Use cases

The following example demonstrates a logical search using a stop word in a query.

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Indexing documents in a document folder
index.add(documentFolder);

// Searching in the index. The word 'of' is the stop word by default.
SearchResult result = index.search("Theory AND of AND relativity");
```

### Implement ability to get status of indexed file

This improvement adds new method to the **DocumentInfo** class, which allow to get the type of the document indexing source and the indexing error indicator.

##### Public API changes

Method **int getDocumentSourceKind()** has been added to **com.groupdocs.search.results.DocumentInfo** class.  
Method **boolean getIndexedWithError()** has been added to **com.groupdocs.search.results.DocumentInfo** class.

##### Use cases

The following example demonstrates how to get the type of source of an indexed document and the indexing error indicator.

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Indexing documents in a document folder
index.add(documentFolder);

// Getting information about indexed documents
DocumentInfo[] infos = index.getIndexedDocuments();
for (DocumentInfo info : infos) {
    Console.writeLine("Document source kind: " + info.getDocumentSourceKind());
    Console.writeLine("Indexed with error: " + info.getIndexedWithError());
}
```

### Implement ability to delete files indexed from stream

This improvement allows to delete documents from an index that have been indexed from a stream or from a structure.

##### Public API changes

Method **DeleteResult delete(UpdateOptions, String[])** has been added to **com.groupdocs.search.Index** class.

##### Use cases

The following example demonstrates how to delete documents from an index by key.

```java
String indexFolder = "c:\\MyIndex";
String documentPath = "c:\\MyDocuments\\Document.pdf";
String documentKey = "My Document.pdf";

// Creating an index
Index index = new Index(indexFolder);

// Indexing a document from stream
File file = new File(documentPath);
try (InputStream stream = new FileInputStream(file)) {
    Document document = Document.createFromStream(documentKey, new Date(System.currentTimeMillis()), ".pdf", stream);
    index.add(new Document[] { document }, new IndexingOptions());
}

// Deleting the document from the index by key
index.delete(new UpdateOptions(), new String[] { documentKey });
```
