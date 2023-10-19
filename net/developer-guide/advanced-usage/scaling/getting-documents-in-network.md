---
id: getting-documents-in-network
url: search/net/getting-documents-in-network
title: Getting documents in network
weight: 120
description: "This page contains information about getting indexed documents in the search network."
keywords: getting indexed documents in network, getting documents in distributed index, getting documents in search network, getting document list in search network
productName: GroupDocs.Search for .NET
hideChildren: False
---
To obtain a list of indexed documents in a specific search network shard, you must call the [GetIndexedDocuments](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/getindexeddocuments/) method of the [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/) class.

To obtain a list of shards of the search network, use the [GetShardIndices](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/getshardindices/) method of the [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) class.

It is also possible to get a list of nested items of indexed container document. To do this, use the [GetIndexedDocumentItems](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/getindexeddocumentitems/) method of the [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/) class.

The following code example demonstrates obtaining a list of documents indexed in the search network, as well as outputting the resulting data to the console.

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
        Console.WriteLine(nodeIndex + ": " + info.ShardIndex + ": " + info.DocumentInfo.FilePath);
        string[] attributes = indexer.GetAttributes(info.DocumentInfo.FilePath);
        for (int k = 0; k < attributes.Length; k++)
        {
            Console.WriteLine("\t\t" + attributes[k]);
        }

        NetworkDocumentInfo[] items = searcher.GetIndexedDocumentItems(info);
        for (int k = 0; k < items.Length; k++)
        {
            NetworkDocumentInfo item = items[k];
            Console.WriteLine("\t" + nodeIndex + ": " + item.ShardIndex + ": " + item.DocumentInfo.ToString());
        }
    }
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
