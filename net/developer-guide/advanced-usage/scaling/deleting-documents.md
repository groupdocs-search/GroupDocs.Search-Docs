---
id: deleting-documents
url: search/net/deleting-documents
title: Deleting documents
weight: 50
description: "This page contains information about deleting documents in the search network."
keywords: search network deleting documents, distributed index removing documents, delete documents in search network
productName: GroupDocs.Search for .NET
hideChildren: False
---
Documents can be deleted from the search network using the [Delete](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/delete/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class.

To delete documents, pass an array of document keys, as well as a delete options object, to the [Delete](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/delete/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class. The keys are the keys that were specified when the documents were [added]({{< ref "search/net/developer-guide/advanced-usage/scaling/indexing-documents.md" >}}).

Deleted documents are excluded from search results immediately, but their data are physically removed from the shards only by [optimization]({{< ref "search/net/developer-guide/advanced-usage/scaling/optimizing-shards.md" >}}).

An asynchronous delete flag is available as an option ([DeleteOptions.IsAsync](https://reference.groupdocs.com/search/net/groupdocs.search.options/deleteoptions/isasync/)). In the case of asynchronous deletion, you can track the completion of the operation by subscribing to the [DeletionCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/deletioncompleted/) event.

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

The following code example, which continues the previous one, demonstrates how to delete documents from the search network. The documents were added from the files of the specified folder with the file names as document keys.

**C#**

```csharp
Console.WriteLine();
Console.WriteLine("Deleting documents");

string[] filePaths = Directory.GetFiles(@"c:\MyDocuments\");
string[] fileNames = new string[filePaths.Length];
for (int i = 0; i < filePaths.Length; i++)
{
    string filePath = filePaths[i];
    string fileName = Path.GetFileName(filePath);
    fileNames[i] = fileName;
}

Indexer indexer = node.Indexer;

DeleteOptions options = new DeleteOptions();
indexer.Delete(fileNames, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
