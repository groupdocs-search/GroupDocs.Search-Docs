---
id: getting-documents-in-network
url: search/java/getting-documents-in-network
title: Getting documents in network
weight: 120
description: "This page contains information about getting indexed documents in the search network."
keywords: getting indexed documents in network, getting documents in distributed index, getting documents in search network, getting document list in search network
productName: GroupDocs.Search for Java
hideChildren: False
---
To obtain a list of indexed documents in a specific search network shard, you must call the [getIndexedDocuments](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#getIndexedDocuments-int-) method of the [Searcher](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/) class.

To obtain a list of shards of the search network, use the [getShardIndices](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searchnetworknode/#getShardIndices--) method of the [SearchNetworkNode](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searchnetworknode/) class.

It is also possible to get a list of nested items of indexed container document. To do this, use the [getIndexedDocumentItems](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#getIndexedDocumentItems-com.groupdocs.search.scaling.results.NetworkDocumentInfo-) method of the [Searcher](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/) class.

The following code example demonstrates obtaining a list of documents indexed in the search network, as well as outputting the resulting data to the console.

```java
Searcher searcher = node.getSearcher();
Indexer indexer = node.getIndexer();

int[] shardIndices = node.getShardIndices();
System.out.println();
for (int i = 0; i < shardIndices.length; i++) {
    int shardIndex = shardIndices[i];
    NetworkDocumentInfo[] infos = searcher.getIndexedDocuments(shardIndex);
    for (NetworkDocumentInfo info : infos) {
        int nodeIndex = node.getNodeIndex(info.getShardIndex());
        System.out.println(nodeIndex + ": " + info.getShardIndex() + ": " + info.getDocumentInfo().getFilePath());
        String[] attributes = indexer.getAttributes(info.getDocumentInfo().getFilePath());
        for (String attribute : attributes) {
            System.out.println("\t\t" + attribute);
        }

        NetworkDocumentInfo[] items = searcher.getIndexedDocumentItems(info);
        for (NetworkDocumentInfo item : items) {
            System.out.println("\t" + nodeIndex + ": " + item.getShardIndex() + ": " + item.getDocumentInfo().toString());
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
