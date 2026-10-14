---
id: scaling
url: search/net/scaling
title: Scaling
weight: 5
description: "This section describes the search network, which distributes an index across several servers."
keywords: search network, scaling, distributed index, shard
productName: GroupDocs.Search for .NET
hideChildren: False
---
A search network distributes indexed data across several servers (nodes) to increase the performance of indexing and searching. The data are stored in shards located on slave nodes, while the master node provides the indexing and search services through which all operations are performed. The classes of the search network are located in the GroupDocs.Search.Scaling namespace and its nested namespaces.

The articles in this section describe the following topics:

*   [Configuring search network]({{< ref "search/net/developer-guide/advanced-usage/scaling/configuring-search-network.md" >}})
*   [Search network deployment]({{< ref "search/net/developer-guide/advanced-usage/scaling/search-network-deployment.md" >}})
*   [Search network node events]({{< ref "search/net/developer-guide/advanced-usage/scaling/search-network-node-events.md" >}})
*   [Indexing documents]({{< ref "search/net/developer-guide/advanced-usage/scaling/indexing-documents.md" >}})
*   [Deleting documents]({{< ref "search/net/developer-guide/advanced-usage/scaling/deleting-documents.md" >}})
*   [Optimizing shards]({{< ref "search/net/developer-guide/advanced-usage/scaling/optimizing-shards.md" >}})
*   [Synchronizing shards]({{< ref "search/net/developer-guide/advanced-usage/scaling/synchronizing-shards.md" >}})
*   [Working with attributes]({{< ref "search/net/developer-guide/advanced-usage/scaling/working-with-attributes.md" >}})
*   [Text search in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/text-search-in-network.md" >}})
*   [Image search in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/image-search-in-network.md" >}})
*   [Highlighting results in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/highlighting-results-in-network.md" >}})
*   [Getting documents in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/getting-documents-in-network.md" >}})
*   [Getting document text in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/getting-document-text-in-network.md" >}})
*   [Managing dictionaries in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/managing-dictionaries-in-network.md" >}})

All nodes of a search network must use the same version of GroupDocs.Search. For upgrading a search network to version 26.10, see the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
