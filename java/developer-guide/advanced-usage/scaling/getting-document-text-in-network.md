---
id: getting-document-text-in-network
url: search/java/getting-document-text-in-network
title: Getting document text in network
weight: 130
description: "This page contains information about getting document text in the search network."
keywords: getting document text in network, getting document text in distributed index, getting document text in search network, getting document content in search network
productName: GroupDocs.Search for Java
hideChildren: False
---
To obtain the extracted text of indexed documents in the search network, the [getDocumentText](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#getDocumentText-com.groupdocs.search.scaling.results.NetworkDocumentInfo-com.groupdocs.search.common.OutputAdapter-) method of the [Searcher](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/) class is used.

The first parameter of the [getDocumentText](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#getDocumentText-com.groupdocs.search.scaling.results.NetworkDocumentInfo-com.groupdocs.search.common.OutputAdapter-) method is a document represented by the [NetworkDocumentInfo](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling.results/networkdocumentinfo/) class. A list of all indexed documents in the search network can be obtained using the [getIndexedDocuments](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#getIndexedDocuments-int-) method of the [Searcher](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/) class.

The second parameter of the [getDocumentText](https://reference.groupdocs.com/search/java/com.groupdocs.search.scaling/searcher/#getDocumentText-com.groupdocs.search.scaling.results.NetworkDocumentInfo-com.groupdocs.search.common.OutputAdapter-) method is the result output adapter. In this operation, the same output adapter classes are used as those used in similar operations of a single index. For information about output adapters, see the article [Output adapters](https://docs.groupdocs.com/search/java/output-adapters/).

The following code example demonstrates retrieving the text of documents indexed in the search network, as well as outputting the resulting data to the console.

```java
Searcher searcher = node.getSearcher();

ArrayList<NetworkDocumentInfo> documents = new ArrayList<NetworkDocumentInfo>();
int[] shardIndices = node.getShardIndices();
for (int i = 0; i < shardIndices.length; i++) {
    int shardIndex = shardIndices[i];
    NetworkDocumentInfo[] infos = searcher.getIndexedDocuments(shardIndex);
    documents.addAll(Arrays.asList(infos));
    for (NetworkDocumentInfo info : infos) {
        NetworkDocumentInfo[] items = searcher.getIndexedDocumentItems(info);
        documents.addAll(Arrays.asList(items));
    }
}

for (int i = 0; i < documents.size(); i++) {
    NetworkDocumentInfo document = documents.get(i);
    if (document.getDocumentInfo().toString().contains(containsInPath)) {
        System.out.println();
        System.out.println(document.getDocumentInfo().toString());

        StringOutputAdapter outputAdapter = new StringOutputAdapter(OutputFormat.PlainText);
        searcher.getDocumentText(document, outputAdapter);

        System.out.println(outputAdapter.getResult());
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
