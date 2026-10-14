---
id: working-with-attributes
url: search/net/working-with-attributes
title: Working with attributes
weight: 80
description: "This page contains information about working with attributes in the search network."
keywords: managing attributes, attribute management, setting attributes, document attributes in search network
productName: GroupDocs.Search for .NET
hideChildren: False
---
Text attributes of documents are changed without the need to re-index documents in the search network.

To change document attributes, use the [ChangeAttributes](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/changeattributes/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class. The changes are described by an [AttributeChangeBatch](https://reference.groupdocs.com/search/net/groupdocs.search.common/attributechangebatch/) object, in which documents are specified by their keys.

The asynchronous operation flag is available as an option ([ChangeAttributesOptions.IsAsync](https://reference.groupdocs.com/search/net/groupdocs.search.options/changeattributesoptions/isasync/)). You can track the completion of the operation by subscribing to the [AttributeChangesCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/attributechangescompleted/) event.

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

The following code example, which continues the previous one, demonstrates changing document attributes on the search network.

**C#**

```csharp
string documentKey = "Lorem ipsum.pdf"; // The key of an indexed document
string attribute = "important";

Console.WriteLine("Adding attribute: " + attribute);

Indexer indexer = node.Indexer;

AttributeChangeBatch batch = new AttributeChangeBatch();
batch.Add(documentKey, attribute);
ChangeAttributesOptions options = new ChangeAttributesOptions();
indexer.ChangeAttributes(batch, options);
```

To get all the attributes of a document, use the [GetAttributes](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getattributes/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class. You must pass the document key as a parameter.

The following code example, which continues the code that creates the master node, demonstrates retrieving document attributes from the search network.

**C#**

```csharp
Searcher searcher = node.Searcher;
Indexer indexer = node.Indexer;

int[] shardIndices = node.GetShardIndices();
Console.WriteLine();
for (int i = 0; i < shardIndices.Length; i++)
{
    int shardIndex = shardIndices[i];
    NetworkDocumentInfo[] infos = searcher.GetIndexedDocuments(shardIndex);
    for (int j = 0; j < infos.Length; j++)
    {
        NetworkDocumentInfo info = infos[j];
        int nodeIndex = node.GetNodeIndex(info.ShardIndex);
        Console.WriteLine(nodeIndex + ": " + info.ShardIndex + ": " +
            info.DocumentInfo.IndexedWithError + ": " + info.DocumentInfo.FilePath);
        string[] attributes = indexer.GetAttributes(info.DocumentInfo.FilePath);
        for (int k = 0; k < attributes.Length; k++)
        {
            Console.WriteLine("\t\t" + attributes[k]);
        }
    }
}
```

More information about working with text attributes of documents can be found on the [Document attributes]({{< ref "search/net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
