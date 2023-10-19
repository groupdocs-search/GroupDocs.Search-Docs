---
id: indexing-documents
url: search/net/indexing-documents
title: Indexing documents
weight: 40
description: "This page contains information about indexing documents in the search network."
keywords: search network indexing, distributed index indexing, adding documents in search network
productName: GroupDocs.Search for .NET
hideChildren: False
---
Documents can be added to the search network using the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class.

The first parameter of the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method is an array of documents to be added. Currently, documents created from a stream or from a structure are supported.

The second parameter of the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method is an array of optional passwords for opening added documents.

The third parameter of the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method is the indexing options object.

The following code example demonstrates adding documents from a stream.

**C#**

```csharp
Stream[] streams = new Stream[filePaths.Length];
Document[] documents = new Document[filePaths.Length];
string[] passwords = new string[filePaths.Length];
for (int i = 0; i < filePaths.Length; i++)
{
    string filePath = filePaths[i];
    DateTime modificationDate = File.GetLastWriteTime(filePath);
    string fileName = Path.GetFileName(filePath);
    string extension = Path.GetExtension(filePath);
    Stream stream = File.OpenRead(filePath);
    streams[i] = stream;
    Document document = Document.CreateFromStream(
        fileName,
        modificationDate,
        extension,
        stream);
    documents[i] = document;
}

Indexer indexer = node.Indexer;

IndexingOptions options = new IndexingOptions();
options.UseRawTextExtraction = false;
options.ImageIndexingOptions.EnabledForSeparateImages = true;
options.ImageIndexingOptions.EnabledForEmbeddedImages = true;
options.ImageIndexingOptions.EnabledForContainerItemImages = true;
options.OcrIndexingOptions.EnabledForSeparateImages = true;
options.OcrIndexingOptions.EnabledForEmbeddedImages = true;
options.OcrIndexingOptions.EnabledForContainerItemImages = true;

indexer.Add(documents, passwords, options);

for (int i = 0; i < streams.Length; i++)
{
    streams[i].Close();
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
