---
id: getting-indexed-documents
url: search/net/getting-indexed-documents
title: Getting indexed documents
weight: 7
description: "This article explains how to get a list of indexed documents from an index, and how to get the text of indexed documents in HTML or plain text format."
keywords: Getting indexed documents
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains a description of how to get a list of indexed documents from an index, and how to get the text of indexed documents in HTML or plain text format.

## Getting indexed documents

To get a list of indexed documents from an index, use the [GetIndexedDocuments](https://reference.groupdocs.com/search/net/groupdocs.search/index/getindexeddocuments/) method of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class. Documents with the extensions ZIP, PST, OST can also contain internal documents. To get a list of internal documents, use the [GetIndexedDocumentItems](https://reference.groupdocs.com/search/net/groupdocs.search/index/getindexeddocumentitems/) method of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class. For ZIP archives, this way you can access documents of arbitrary nesting depth. An example of obtaining a list of documents from an index is presented below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Getting list of indexed documents
DocumentInfo[] documents = index.GetIndexedDocuments();
for (int i = 0; i < documents.Length; i++)
{
    DocumentInfo document = documents[i];
    Console.WriteLine(document.FilePath);
    DocumentInfo[] items = index.GetIndexedDocumentItems(document); // Getting list of document items
    for (int j = 0; j < items.Length; j++)
    {
        DocumentInfo item = items[j];
        Console.WriteLine("\t" + item.InnerPath);
    }
}
```

To get a single indexed document, use the [GetIndexedDocument](https://reference.groupdocs.com/search/net/groupdocs.search/index/getindexeddocument/) method. It takes the file path of a document indexed from a file (as returned by the [DocumentInfo.FilePath](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/filepath/) property) or the key of a document indexed from a stream or a structure, and returns null if there is no such document in the index.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Getting an indexed document by its file path
DocumentInfo document = index.GetIndexedDocument(@"c:\MyDocuments\Lorem ipsum.docx");
if (document != null)
{
    Console.WriteLine(document.FilePath + " (" + document.FileType + ")");
}
```

## Getting text of indexed documents

The text of an indexed document can be extracted from an index if the option to save the text of documents in the index was enabled. If this option was not enabled when creating an index, then when the [GetDocumentText](https://reference.groupdocs.com/search/net/groupdocs.search/index/getdocumenttext/) method of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class is called, the text of the document will be extracted from the document again. Details about saving the text of documents in an index can be found on the page [Storing text of indexed documents]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}).

The generated text of the document is passed to an instance of a class derived from the abstract class [OutputAdapter](https://reference.groupdocs.com/search/net/groupdocs.search.common/outputadapter/). Details on the output adapters are presented on the page [Output adapters]({{< ref "search/net/developer-guide/advanced-usage/searching/output-adapters.md" >}}).

After generating the text of a document into a file, this file can be opened by an Internet browser. The following example shows how to extract document text from an index.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Getting list of indexed documents
DocumentInfo[] documents = index.GetIndexedDocuments();
 
// Getting a document text
if (documents.Length > 0)
{
    FileOutputAdapter outputAdapter = new FileOutputAdapter(OutputFormat.Html, @"C:\Text.html");
    index.GetDocumentText(documents[0], outputAdapter);
}
```

To extract the text of a document from an index, there is also an overload of the method that takes an instance of the [TextOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/) class as a parameter. In this class, the following options can be specified:

*   [CustomExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/customextractor/) is a custom extractor used during indexing, it is necessary if the text of the document was not saved in the index;
*   [AdditionalFields](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/additionalfields/) are additional document fields added during document indexing which are also necessary if the document text was not saved in the index;
*   [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/cancellation/) is an object used to cancel the operation;
*   [GenerateHead](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/generatehead/) is a flag to specify whether the Head tag is generated in the output HTML; the default value is true;
*   [MetadataIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/metadataindexingoptions/) is an object for specifying metadata indexing options. This property and the other properties inherited from the [BaseOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/) class (for example, [Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/encoding/), [AutoDetectEncoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/autodetectencoding/) and [OcrIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/ocrindexingoptions/)) specify how the text of the document is extracted; they are necessary if the document text was not saved in the index.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
