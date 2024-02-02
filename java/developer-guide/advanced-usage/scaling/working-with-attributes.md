---
id: working-with-attributes
url: search/java/working-with-attributes
title: Working with attributes
weight: 80
description: "This page contains information about working with attributes in the search network."
keywords: managing attributes, attribute management, setting attributes, document attributes in search network
productName: GroupDocs.Search for Java
hideChildren: False
---
Text attributes of documents are changed without the need to re-index documents in the search network.

To change document attributes, use the [changeAttributes](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/#changeAttributes-com.groupdocs.search.common.AttributeChangeBatch-com.groupdocs.search.options.ChangeAttributesOptions-) method of the [Indexer](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/) class.

The asynchronous operation flag is available as an option. You can track the completion of the operation by subscribing to the [AttributeChangesCompleted](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.events/nodeeventhub/#AttributeChangesCompleted) event.

The following code example demonstrates changing document attributes on the search network.

```java
System.out.println("Adding attribute: " + attribute);

Indexer indexer = node.getIndexer();

AttributeChangeBatch batch = new AttributeChangeBatch();
batch.add(documentKey, attribute);
ChangeAttributesOptions options = new ChangeAttributesOptions();
indexer.changeAttributes(batch, options);
```

To get all the attributes of a document, use the [getAttributes](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/#getAttributes-java.lang.String-) method of the [Indexer](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/indexer/) class. You must pass the document key as a parameter.

The following code example demonstrates retrieving document attributes from the search network.

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
        System.out.println(nodeIndex + ": " + info.getShardIndex() + ": " +
                hasErrors(info) + ": " + info.getDocumentInfo().getFilePath());
        String[] attributes = indexer.getAttributes(info.getDocumentInfo().getFilePath());
        for (String attribute1 : attributes) {
            System.out.println("\t\t" + attribute1);
        }
    }
}
```

More information about working with text attributes of documents in the article [Document attributes](https://docs.groupdocs.com/search/java/document-attributes/).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
