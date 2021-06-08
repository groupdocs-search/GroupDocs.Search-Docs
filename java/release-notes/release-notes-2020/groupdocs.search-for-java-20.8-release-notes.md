---
id: groupdocs-search-for-java-20-8-release-notes
url: search/java/groupdocs-search-for-java-20-8-release-notes
title: GroupDocs.Search for Java 20.8 Release Notes
weight: 3
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 20.8{{< /alert >}}

## Major Features

There are the following features and improvementes in this release:

- Implement indexing from stream
- Implement support for indexing .DICOM files

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-1932 | Implement indexing from stream | New Feature |
| SEARCHNET-2362 | Implement support for indexing .DICOM files | Improvement |

## Public API and Backward Incompatible Changes

### Implement indexing from stream

This feature provides the ability to index documents from streams and data structures.

##### Public API changes

Class **DocumentSourceKind** has been added to **com.groupdocs.search.common** package.  
Field **File** has been added to **com.groupdocs.search.common.DocumentSourceKind** class.  
Field **Stream** has been added to **com.groupdocs.search.common.DocumentSourceKind** class.  
Field **Structure** has been added to **com.groupdocs.search.common.DocumentSourceKind** class.

Class **Document** has been added to **com.groupdocs.search** package.  
Method **int getDocumentSourceKind()** has been added to **com.groupdocs.search.Document** class.  
Method **String getDocumentKey()** has been added to **com.groupdocs.search.Document** class.  
Method **boolean isLazy()** has been added to **com.groupdocs.search.Document** class.  
Method **Date getModificationDate()** has been added to **com.groupdocs.search.Document** class.  
Method **String getExtension()** has been added to **com.groupdocs.search.Document** class.  
Method **DocumentField[] getAdditionalFields()** has been added to **com.groupdocs.search.Document** class.  
Method **void setAdditionalFields(DocumentField[])** has been added to **com.groupdocs.search.Document** class.  
Method **String[] getAttributes()** has been added to **com.groupdocs.search.Document** class.  
Method **void setAttributes(String[])** has been added to **com.groupdocs.search.Document** class.  
Static method **Document createFromFile(String)** has been added to **com.groupdocs.search.Document** class.  
Static method **Document createFromStream(String, Date, String, InputStream)** has been added to **com.groupdocs.search.Document** class.  
Static method **Document createFromStructure(String, Date, DocumentField[])** has been added to **com.groupdocs.search.Document** class.  
Static method **Document createLazy(int, String, IDocumentLoader)** has been added to **com.groupdocs.search.Document** class.

Interface **IDocumentLoader** has been added to **com.groupdocs.search.common** package.  
Method **Document loadDocument()** has been added to **com.groupdocs.search.common.IDocumentLoader** interface.  
Method **void closeDocument()** has been added to **com.groupdocs.search.common.IDocumentLoader** interface.

Method **Document getDocument()** has been added to **com.groupdocs.search.events.FileIndexingEventArgs** class.  
Method **String getDocumentKey()** has been added to **com.groupdocs.search.events.FileIndexingEventArgs** class.

Method **String getLastDocumentKey()** has been added to **com.groupdocs.search.events.OperationProgressEventArgs** class.

Method **ExtractedItemInfo[] getContainerItems(InputStream)** has been added to **com.groupdocs.search.common.IContainerItemExtractor** interface.

Method **DocumentField[] getFields(InputStream)** has been added to **com.groupdocs.search.common.IFieldExtractor** interface.

Method void **add(Document[], IndexingOptions)** has been added to **com.groupdocs.search.Index** class.

##### Use cases

The following example demonstrates how to index document from a stream.


```java
String indexFolder = "c:\\MyIndex";
String documentFilePath = "c:\\MyDocuments\\ExampleDocument.pdf";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Creating a document object
InputStream stream = new FileInputStream(documentFilePath);
Document document = Document.createFromStream(documentFilePath, new Date(), ".pdf", stream);
Document[] documents = new Document[] {
    document,
};
 
// Indexing document from the stream
IndexingOptions options = new IndexingOptions();
index.add(documents, options);
 
// Closing the document stream after indexing is complete
stream.close();
```

### Implement support for indexing .DICOM files

This improvement adds support for indexing .DICOM files.

##### Public API changes

Static field **FileType DICOM** has been added to **com.groupdocs.search.results.FileType** class.

##### Use cases

None.

