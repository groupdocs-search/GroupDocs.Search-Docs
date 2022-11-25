---
id: indexing-from-different-sources
url: search/java/indexing-from-different-sources
title: Indexing from different sources
weight: 10
description: "GroupDocs.Search allows indexing documents from various sources."
keywords: indexing documents from various sources, indexing documents
productName: GroupDocs.Search for Java
hideChildren: False
---
GroupDocs.Search allows indexing documents from various sources:

- From files in the file system.
- From a stream.
- From a data structure as an array of fields.

The library also allows indexing from all presented sources with lazy initialization.

Please note that the update operation automatically generates a list of changed files only when indexing from the local file system. When indexing from streams or structures, documents cannot be updated with the update operation. To update documents from these sources, you must re-index the modified documents by passing their keys and updated data to the [add](https://reference.groupdocs.com/search/java/com.groupdocs.search/Index#add(com.groupdocs.search.Document[],%20com.groupdocs.search.options.IndexingOptions)) method.

## Indexing from a file

It should be borne in mind that the [add](https://reference.groupdocs.com/search/java/com.groupdocs.search/Index#add(com.groupdocs.search.Document[],%20com.groupdocs.search.options.IndexingOptions)) method with the parameter of type [Document](https://reference.groupdocs.com/search/java/com.groupdocs.search/Document)[] allows indexing only documents individually, and not entire folders. The advantage of using this method overload is that you can add attributes and additional fields to the indexed document before calling the [add](https://reference.groupdocs.com/search/java/com.groupdocs.search/Index#add(com.groupdocs.search.Document[],%20com.groupdocs.search.options.IndexingOptions)) method. The following example demonstrates how to index a document from a file.

**Java**

```java
String indexFolder = "c:\\MyIndex";
String documentFilePath = "c:\\MyDocuments\\ExampleDocument.pdf"";

// Creating an index
IndexSettings settings = new IndexSettings();
settings.setUseRawTextExtraction(false);
Index index = new Index(indexFolder, settings);

// Creating a document object
Document document = Document.createFromFile(documentFilePath);
Document[] documents = new Document[] {
    document,
};

// Indexing document from the file
IndexingOptions options = new IndexingOptions();
index.add(documents, options);
```

## Indexing from a stream

The following example demonstrates how to index a document from a stream.

**Java**

```java
String indexFolder = "c:\\MyIndex";
String documentFilePath = "c:\\MyDocuments\\ExampleDocument.pdf";

// Creating an index
Index index = new Index(indexFolder);

// Creating a document object
InputStream stream = new FileInputStream(documentFilePath); // Opening a stream
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

## Indexing from a structure

The following example demonstrates how to index a document from a structure.

**Java**

```java
String indexFolder = "c:\\MyIndex";
String documentFilePath = "c:\\MyDocuments\\ExampleDocument.txt";

// Creating an index
Index index = new Index(indexFolder);

// Creating a document object
String text = new String(Files.readAllBytes(Paths.get(documentFilePath)), StandardCharsets.UTF_8);
DocumentField[] fields = new DocumentField[] {
    new DocumentField(CommonFieldNames.Content, text),
};
Document document = Document.createFromStructure("ExampleDocument", new Date(), fields);
Document[] documents = new Document[] {
    document,
};

// Indexing document from the structure
IndexingOptions options = new IndexingOptions();
index.add(documents, options);
```

## Indexing from URL

The following example demonstrates how to index a document by URL when lazy initialized.

**Java**

```java
private static class DocumentLoaderFromUrl implements IDocumentLoader {
    private final String documentKey;
    private final String url;
    private final String extension;

    public DocumentLoaderFromUrl(String documentKey, String url, String extension) {
        this.documentKey = documentKey;
        this.url = url;
        this.extension = extension;
    }

    @Override
    public final Document loadDocument() {
        try {
            URL urlInstance = new URL(url);
            InputStream stream = urlInstance.openStream();
            Document document = Document.createFromStream(documentKey, new Date(), extension, stream);
            return document;
        } catch (java.io.IOException ex) {
            throw new RuntimeException(ex);
        }
    }

    @Override
    public final void closeDocument() {
    }
}


String indexFolder = "c:\\MyIndex";
String url = "http://example.com/ExampleDocument.pdf";

// Creating an index
IndexSettings settings = new IndexSettings();
settings.setTextStorageSettings(new TextStorageSettings(Compression.High));
settings.setUseRawTextExtraction(false);
Index index = new Index(indexFolder, settings, true);

index.getEvents().ErrorOccurred.add(new EventHandler<IndexErrorEventArgs>() {
    @Override
    public void invoke(Object s, IndexErrorEventArgs a) {
        System.out.println(a.getMessage());
    }
});

// Creating a document object
String documentKey = url;
IDocumentLoader documentLoader = new DocumentLoaderFromUrl(documentKey, url, ".pdf");
Document document = Document.createLazy(DocumentSourceKind.Stream, documentKey, documentLoader);
Document[] documents = new Document[] {
    document,
};

// Indexing the lazy-loaded document
IndexingOptions options = new IndexingOptions();
index.add(documents, options);
```

## Indexing from FTP

The following example demonstrates how to index a document from FTP when lazy initialized.

**Java**

```java
private static class DocumentLoaderFromUrl implements IDocumentLoader {
    private final String documentKey;
    private final String url;
    private final String extension;

    public DocumentLoaderFromUrl(String documentKey, String url, String extension) {
        this.documentKey = documentKey;
        this.url = url;
        this.extension = extension;
    }

    @Override
    public final Document loadDocument() {
        try {
            URL urlInstance = new URL(url);
            InputStream stream = urlInstance.openStream();
            Document document = Document.createFromStream(documentKey, new Date(), extension, stream);
            return document;
        } catch (java.io.IOException ex) {
            throw new RuntimeException(ex);
        }
    }

    @Override
    public final void closeDocument() {
    }
}


String indexFolder = "c:\\MyIndex";
String url = "ftp://example.com/ExampleDocument.pdf";

// Creating an index
IndexSettings settings = new IndexSettings();
settings.setTextStorageSettings(new TextStorageSettings(Compression.High));
settings.setUseRawTextExtraction(false);
Index index = new Index(indexFolder, settings, true);

index.getEvents().ErrorOccurred.add(new EventHandler<IndexErrorEventArgs>() {
    @Override
    public void invoke(Object s, IndexErrorEventArgs a) {
        System.out.println(a.getMessage());
    }
});

// Creating a document object
String documentKey = url;
IDocumentLoader documentLoader = new DocumentLoaderFromUrl(documentKey, url, ".pdf");
Document document = Document.createLazy(DocumentSourceKind.Stream, documentKey, documentLoader);
Document[] documents = new Document[] {
    document,
};

// Indexing the lazy-loaded document
IndexingOptions options = new IndexingOptions();
index.add(documents, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
