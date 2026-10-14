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
Documents can be added to the search network using the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class. The indexer is available through the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/indexer/) property of the master node.

The first parameter of the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method is a list of documents to be added. Only documents created from a stream or from a structure are supported. The key of a document identifies it in the search network; it is used, for example, to delete the document or change its attributes.

The second parameter of the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method is a list of passwords for opening the added documents. The list must have the same length as the list of documents; use *null* for a document without a password.

The third parameter of the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method is the indexing options object. By default, the method returns when indexing is complete. If the [IsAsync](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/isasync/) option is set to *true*, the method returns immediately, and the completion of indexing can be tracked by subscribing to the [IndexingCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/indexingcompleted/) event. Text recognition in images requires an OCR connector set on the nodes with extraction services by the [SetOcrConnector](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/setocrconnector/) method.

The current state of the search network can be obtained with the [Status](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/status/) property of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class.

The examples on this page use the master node of a running search network. The following code creates and starts the master node as described on the [Search network deployment]({{< ref "search/net/developer-guide/advanced-usage/scaling/search-network-deployment.md" >}}) page; the slave nodes must be started beforehand.

**C#**

```csharp
string basePath = @"c:\SearchNetwork\";
int basePort = 49100;

// Loading the search network configuration from a file
Configuration configuration = Configuration.Load(basePath + "Configuration.xml");

// Creating, configuring and starting the master node; the timeouts are specified in milliseconds
SearchNetworkNode node = SearchNetworkNode.CreateMasterNode(
    0,
    basePath + "Node0",
    new TcpSettings(basePort, 3000, 3000, 3000, 1000),
    new FileLogger(basePath + "Log.txt", 4.0),
    configuration);
node.ConfigureAllNodes();
node.Start();
```

The following code example, which continues the previous one, demonstrates adding documents from streams.

**C#**

```csharp
string[] filePaths = Directory.GetFiles(@"c:\MyDocuments\");

Stream[] streams = new Stream[filePaths.Length];
Document[] documents = new Document[filePaths.Length];
string[] passwords = new string[filePaths.Length]; // The documents are not protected with passwords
for (int i = 0; i < filePaths.Length; i++)
{
    string filePath = filePaths[i];
    DateTime modificationDate = File.GetLastWriteTime(filePath);
    string fileName = Path.GetFileName(filePath); // The file name is used as the document key
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

// Indexing the documents; the method returns when indexing is complete
indexer.Add(documents, passwords, options);

for (int i = 0; i < streams.Length; i++)
{
    streams[i].Close();
}
```

## Extracting data separately from indexing

The [Extract](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/extract/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class only extracts data from documents using the extraction services of the network, without indexing. The data extracted from each document are passed to the [DataExtracted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/dataextracted/) event, and the completion of the operation is reported by the [ExtractionCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/extractioncompleted/) event. The [IsAsync](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/isasync/) option has the same meaning as for the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method.

The extracted data can then be indexed with the overload of the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/add/) method that accepts a list of [ExtractedData](https://reference.groupdocs.com/search/net/groupdocs.search.common/extracteddata/) objects. This overload does not use the extraction services.

The following code example, which continues the code that creates the master node, demonstrates extracting data from documents and indexing the extracted data.

**C#**

```csharp
string[] filePaths = Directory.GetFiles(@"c:\MyDocuments\");

Stream[] streams = new Stream[filePaths.Length];
Document[] documents = new Document[filePaths.Length];
string[] passwords = new string[filePaths.Length];
for (int i = 0; i < filePaths.Length; i++)
{
    string filePath = filePaths[i];
    Stream stream = File.OpenRead(filePath);
    streams[i] = stream;
    documents[i] = Document.CreateFromStream(
        Path.GetFileName(filePath),
        File.GetLastWriteTime(filePath),
        Path.GetExtension(filePath),
        stream);
}

// Collecting the extracted data
List<ExtractedData> extractedData = new List<ExtractedData>();
node.Events.DataExtracted += (sender, args) =>
{
    lock (extractedData)
    {
        extractedData.Add(args.Data);
    }
};
node.Events.ExtractionCompleted += (sender, args) =>
{
    Console.WriteLine("Extraction complete");
};

Indexer indexer = node.Indexer;

// Extracting data from the documents; the method returns when extraction is complete
indexer.Extract(documents, passwords, new IndexingOptions());

for (int i = 0; i < streams.Length; i++)
{
    streams[i].Close();
}

// Indexing the extracted data
indexer.Add(extractedData, new IndexingOptions());
```

## Deleting all data

To delete all indexed data from all shards of the search network, use the [DeleteAllData](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/deletealldata/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class. The method returns when all shards have deleted their data.

**C#**

```csharp
// Deleting all indexed data from the search network (continues the code that creates the master node)
node.Indexer.DeleteAllData();
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
