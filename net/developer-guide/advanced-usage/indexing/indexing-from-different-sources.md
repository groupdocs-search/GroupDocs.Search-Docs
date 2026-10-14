---
id: indexing-from-different-sources
url: search/net/indexing-from-different-sources
title: Indexing from different sources
weight: 13
description: "This article explains how to index documents from files, streams and structures, as well as lazy-loaded documents from URL, FTP, Amazon S3 and Azure Blob Storage."
keywords: indexing documents from various sources, index from stream, index from structure, lazy loading, IDocumentLoader
productName: GroupDocs.Search for .NET
hideChildren: False
---

GroupDocs.Search allows indexing documents from various sources:

- From files in the file system.
- From a stream.
- From a data structure as an array of fields.

The library also allows indexing from all presented sources with lazy initialization: a document created with the [Document.CreateLazy](https://reference.groupdocs.com/search/net/groupdocs.search.common/document/createlazy/) method is loaded by an [IDocumentLoader](https://reference.groupdocs.com/search/net/groupdocs.search.common/idocumentloader/) implementation only when the index processes it. The [LoadDocument](https://reference.groupdocs.com/search/net/groupdocs.search.common/idocumentloader/loaddocument/) method of the loader returns the loaded document, and the [CloseDocument](https://reference.groupdocs.com/search/net/groupdocs.search.common/idocumentloader/closedocument/) method is called to release its resources.

Please note that the update operation automatically generates a list of changed files only when indexing from the local file system. When indexing from streams or structures, documents cannot be updated with the update operation. To update documents from these sources, you must re-index the modified documents by passing their keys and updated data to the [Add](https://reference.groupdocs.com/search/net/groupdocs.search/index/add/) method.

## Indexing from a file

It should be borne in mind that the [Add](https://reference.groupdocs.com/search/net/groupdocs.search/index/add/) method with the parameter of type [Document](https://reference.groupdocs.com/search/net/groupdocs.search.common/document/)[] allows indexing only documents individually, and not entire folders. The advantage of using this method overload is that you can add attributes and additional fields to the indexed document before calling the [Add](https://reference.groupdocs.com/search/net/groupdocs.search/index/add/) method. The following example demonstrates how to index a document from a file.

**C#**

```csharp
string indexFolder = @"c:\MyIndex";
string documentFilePath = @"c:\MyDocuments\ExampleDocument.pdf";
  
// Creating an index
Index index = new Index(indexFolder);
  
// Creating a document object
Document document = Document.CreateFromFile(documentFilePath);
Document[] documents = new Document[]
{
    document,
};
  
// Indexing document from the file
IndexingOptions options = new IndexingOptions();
index.Add(documents, options);
```

## Indexing from a stream

The following example demonstrates how to index a document from a stream.

**C#**

```csharp
string indexFolder = @"c:\MyIndex";
string documentFilePath = @"c:\MyDocuments\ExampleDocument.pdf";
  
// Creating an index
Index index = new Index(indexFolder);
  
// Creating a document object
Stream stream = File.OpenRead(documentFilePath); // Opening a stream
Document document = Document.CreateFromStream(documentFilePath, DateTime.Now, ".pdf", stream);
Document[] documents = new Document[]
{
    document,
};
  
// Indexing document from the stream
IndexingOptions options = new IndexingOptions();
index.Add(documents, options);
  
// Closing the document stream after indexing is complete
stream.Close();
```

## Indexing from a structure

The following example demonstrates how to index a document from a structure.

**C#**

```csharp
string indexFolder = @"c:\MyIndex";
string documentFilePath = @"c:\MyDocuments\ExampleDocument.txt";
  
// Creating an index
Index index = new Index(indexFolder);
  
// Creating a document object
string text = File.ReadAllText(documentFilePath);
DocumentField[] fields = new DocumentField[]
{
    new DocumentField(CommonFieldNames.Content, text),
};
Document document = Document.CreateFromStructure("ExampleDocument", DateTime.Now, fields);
Document[] documents = new Document[]
{
    document,
};
  
// Indexing document from the structure
IndexingOptions options = new IndexingOptions();
index.Add(documents, options);
```

## Indexing from URL

The following example demonstrates how to index a document by URL when lazy initialized.

**C#**

```csharp
public class DocumentLoaderFromUrl : IDocumentLoader
{
    // A single HttpClient instance is reused for all requests
    private static readonly HttpClient httpClient = new HttpClient();

    private readonly string documentKey;
    private readonly string url;
    private readonly string extension;
  
    public DocumentLoaderFromUrl(string documentKey, string url, string extension)
    {
        this.documentKey = documentKey;
        this.url = url;
        this.extension = extension;
    }
  
    public Document LoadDocument()
    {
        // HttpClient uses the security protocols allowed by the operating system (TLS 1.2 or later)
        // On .NET Framework, TLS 1.2 may have to be enabled explicitly:
        // ServicePointManager.SecurityProtocol |= SecurityProtocolType.Tls12;
        byte[] data = httpClient.GetByteArrayAsync(url).GetAwaiter().GetResult();
        MemoryStream memoryStream = new MemoryStream(data);
        Document document = Document.CreateFromStream(documentKey, DateTime.Now, extension, memoryStream);
        return document;
    }
  
    public void CloseDocument()
    {
    }
}
 
 
string indexFolder = @"c:\MyIndex";
string url = "https://example.com/ExampleDocument.pdf";
  
// Creating an index
Index index = new Index(indexFolder);
  
// Creating a document object
string documentKey = url;
IDocumentLoader documentLoader = new DocumentLoaderFromUrl(documentKey, url, ".pdf");
Document document = Document.CreateLazy(DocumentSourceKind.Stream, documentKey, documentLoader);
Document[] documents = new Document[]
{
    document,
};
  
// Indexing the lazy-loaded document
IndexingOptions options = new IndexingOptions();
index.Add(documents, options);
```

## Indexing from FTP

The following example demonstrates how to index a document from FTP when lazy initialized.

**C#**

```csharp
public class DocumentLoaderFromFtp : IDocumentLoader
{
    private readonly string documentKey;
    private readonly string url;
    private readonly string extension;
  
    public DocumentLoaderFromFtp(string documentKey, string url, string extension)
    {
        this.documentKey = documentKey;
        this.url = url;
        this.extension = extension;
    }
  
    public Document LoadDocument()
    {
        FtpWebRequest request = (FtpWebRequest)WebRequest.Create(url);
        using (WebResponse response = request.GetResponse())
        using (Stream stream = response.GetResponseStream())
        {
            MemoryStream memoryStream = new MemoryStream();
            stream.CopyTo(memoryStream);
            memoryStream.Position = 0;
            Document document = Document.CreateFromStream(documentKey, DateTime.Now, extension, memoryStream);
            return document;
        }
    }
  
    public void CloseDocument()
    {
    }
}
 
 
string indexFolder = @"c:\MyIndex";
string url = "ftp://example.com/ExampleDocument.pdf";
  
// Creating an index
Index index = new Index(indexFolder);
  
// Creating a document object
string documentKey = url;
IDocumentLoader documentLoader = new DocumentLoaderFromFtp(documentKey, url, ".pdf");
Document document = Document.CreateLazy(DocumentSourceKind.Stream, documentKey, documentLoader);
Document[] documents = new Document[]
{
    document,
};
  
// Indexing the lazy-loaded document
IndexingOptions options = new IndexingOptions();
index.Add(documents, options);
```

## Indexing from Amazon S3 Storage

The following example demonstrates how to index a document from Amazon S3 Storage when lazy initialized. The example uses the [AWSSDK.S3](https://www.nuget.org/packages/AWSSDK.S3/) package.

**C#**

```csharp
public class DocumentLoaderFromAmazon : IDocumentLoader
{
    private readonly string documentKey;
    private readonly string extension;
    private readonly string storageKey;
  
    public DocumentLoaderFromAmazon(string documentKey, string extension, string storageKey)
    {
        this.documentKey = documentKey;
        this.extension = extension;
        this.storageKey = storageKey;
    }
  
    public Document LoadDocument()
    {
        AmazonS3Client client = new AmazonS3Client();
        string bucketName = "my-bucket";
        GetObjectRequest request = new GetObjectRequest
        {
            Key = storageKey,
            BucketName = bucketName,
        };
        using (GetObjectResponse response = client.GetObjectAsync(request).GetAwaiter().GetResult())
        {
            MemoryStream memoryStream = new MemoryStream();
            response.ResponseStream.CopyTo(memoryStream);
            memoryStream.Position = 0;
            Document document = Document.CreateFromStream(documentKey, DateTime.Now, extension, memoryStream);
            return document;
        }
    }
  
    public void CloseDocument()
    {
    }
}
 
 
string indexFolder = @"c:\MyIndex";
string key = "example.pdf";
  
// Creating an index
Index index = new Index(indexFolder);
  
// Creating a document object
IDocumentLoader documentLoader = new DocumentLoaderFromAmazon("documentKey", ".pdf", key);
Document document = Document.CreateLazy(DocumentSourceKind.Stream, "documentKey", documentLoader);
Document[] documents = new Document[]
{
    document,
};
  
// Indexing the lazy-loaded document
IndexingOptions options = new IndexingOptions();
index.Add(documents, options);
```

## Indexing from Azure Blob Storage

The following example demonstrates how to index a document from Azure Blob Storage when lazy initialized. The example uses the [Azure.Storage.Blobs](https://www.nuget.org/packages/Azure.Storage.Blobs/) package.

**C#**

```csharp
public class DocumentLoaderFromAzure : IDocumentLoader
{
    private readonly string documentKey;
    private readonly string extension;
    private readonly string blobName;
  
    public DocumentLoaderFromAzure(string documentKey, string extension, string blobName)
    {
        this.documentKey = documentKey;
        this.extension = extension;
        this.blobName = blobName;
    }
  
    public Document LoadDocument()
    {
        string connectionString = "***"; // The connection string of the storage account
        string containerName = "***";
        BlobClient blobClient = new BlobClient(connectionString, containerName, blobName);

        MemoryStream memoryStream = new MemoryStream();
        blobClient.DownloadTo(memoryStream);
        memoryStream.Position = 0;
  
        Document document = Document.CreateFromStream(documentKey, DateTime.Now, extension, memoryStream);
        return document;
    }
  
    public void CloseDocument()
    {
    }
}
 
 
string indexFolder = @"c:\MyIndex";
string blobName = "example.pdf";
  
// Creating an index
Index index = new Index(indexFolder);
  
// Creating a document object
IDocumentLoader documentLoader = new DocumentLoaderFromAzure("documentKey", ".pdf", blobName);
Document document = Document.CreateLazy(DocumentSourceKind.Stream, "documentKey", documentLoader);
Document[] documents = new Document[]
{
    document,
};
  
// Indexing the lazy-loaded document
IndexingOptions options = new IndexingOptions();
index.Add(documents, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
