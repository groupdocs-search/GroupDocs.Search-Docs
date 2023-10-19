---
id: getting-document-text-in-network
url: search/net/getting-document-text-in-network
title: Getting document text in network
weight: 130
description: "This page contains information about getting document text in the search network."
keywords: getting document text in network, getting document text in distributed index, getting document text in search network, getting document content in search network
productName: GroupDocs.Search for .NET
hideChildren: False
---
To obtain the extracted text of indexed documents in the search network, the [GetDocumentText](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/getdocumenttext/) method of the [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/) class is used.

The first parameter of the [GetDocumentText](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/getdocumenttext/) method is a document represented by the [NetworkDocumentInfo](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.results/networkdocumentinfo/) class. A list of all indexed documents in the search network can be obtained using the [GetIndexedDocuments](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/getindexeddocuments/) method of the [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/) class.

The second parameter of the [GetDocumentText](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/getdocumenttext/) method is the result output adapter. In this operation, the same output adapter classes are used as those used in similar operations of a single index. For information about output adapters, see the article [Output adapters](https://docs.groupdocs.com/search/net/output-adapters/).

The following code example demonstrates retrieving the text of documents indexed in the search network, as well as outputting the resulting data to the console.

**C#**

```csharp
Searcher searcher = node.Searcher;

List<NetworkDocumentInfo> documents = new List<NetworkDocumentInfo>();
int[] shardIndices = node.GetShardIndices();
for (int i = 0; i < shardIndices.Length; i++)
{
    int shardIndex = shardIndices[i];
    NetworkDocumentInfo[] infos = searcher.GetIndexedDocuments(shardIndex);
    documents.AddRange(infos);
    for (int j = 0; j < infos.Length; j++)
    {
        NetworkDocumentInfo info = infos[j];
        NetworkDocumentInfo[] items = searcher.GetIndexedDocumentItems(info);
        documents.AddRange(items);
    }
}

for (int i = 0; i < documents.Count; i++)
{
    NetworkDocumentInfo document = documents[i];
    if (document.DocumentInfo.ToString().Contains(containsInPath))
    {
        Console.WriteLine();
        Console.WriteLine(document.DocumentInfo.ToString());

        StringOutputAdapter outputAdapter = new StringOutputAdapter(OutputFormat.PlainText);
        searcher.GetDocumentText(document, outputAdapter);

        Console.WriteLine(outputAdapter.GetResult());
        break;
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
