---
id: delete-indexed-documents
url: search/java/delete-indexed-documents
title: Delete indexed documents
weight: 4
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
GroupDocs.Search has the ability to remove individual documents from the index that are indexed from a stream or structure. To remove documents indexed by a file system path, see [Deleting indexed paths]({{< ref "search/java/developer-guide/advanced-usage/indexing/delete-indexed-paths.md" >}}).

The following example shows how to remove documents from an index by document key.



```java
// Implementing document loader
private static class DocumentLoader implements IDocumentLoader {
    private final String filePath;
    private final String documentKey;

    public DocumentLoader(String filePath) {
        this.filePath = filePath;
        documentKey = FilenameUtils.getName(filePath);
    }

    public final String getDocumentKey() {
        return documentKey;
    }

    public final void closeDocument() {
    }

    public final Document loadDocument() {
        try {
            String extension = "." + FilenameUtils.getExtension(filePath);
            Path path = Paths.get(filePath);
            byte[] buffer = Files.readAllBytes(path);
            ByteArrayInputStream stream = new ByteArrayInputStream(buffer);
            Document document = Document.createFromStream(documentKey, new Date(System.currentTimeMillis()), extension, stream);
            return document;
        }
        catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}

...

String filePath = "c:\\MyDocuments\\SomeDocument.pdf";
String indexFolder = "c:\\MyIndex\\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing the document from stream
DocumentLoader documentLoader = new DocumentLoader(filePath);
Document document = Document.createLazy(DocumentSourceKind.Stream, documentLoader.getDocumentKey(), documentLoader);
Document[] documents = new Document[] { document };
index.add(documents, new IndexingOptions());

// Searching in the index
SearchResult searchResult = index.search("Einstein");

// Deleting indexed document from the index
String[] documentKeys = new String[] { documentLoader.getDocumentKey() };
DeleteResult deleteResult = index.delete(new UpdateOptions(), documentKeys);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
