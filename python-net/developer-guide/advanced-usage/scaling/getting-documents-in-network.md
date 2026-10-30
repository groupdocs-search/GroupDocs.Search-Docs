---
id: getting-documents-in-network
url: search/python-net/getting-documents-in-network
title: Getting documents in network
weight: 120
description: "This page contains information about getting indexed documents in the search network."
keywords: getting indexed documents in network, getting documents in distributed index, getting documents in search network, getting document list in search network
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
To obtain a list of indexed documents in a specific search network shard, call the [get_indexed_documents](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/get_indexed_documents/) method of the [Searcher](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/) class. The method returns a list of [NetworkDocumentInfo](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkdocumentinfo/) objects; each of them contains the information about the document ([document_info](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkdocumentinfo/document_info/) property) and the index of the shard that contains it ([shard_index](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkdocumentinfo/shard_index/) property).

To obtain a list of shards of the search network, use the [get_shard_indices](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/get_shard_indices/) method of the [SearchNetworkNode](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/) class. To get the index of the node on which a shard is located, use the [get_node_index](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/get_node_index/) method.

It is also possible to get a list of nested items of an indexed container document. To do this, use the [get_indexed_document_items](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/get_indexed_document_items/) method of the [Searcher](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/) class.

The examples on this page use the master node of a running search network. The following code creates and starts the master node as described on the [Search network deployment]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/search-network-deployment.md" >}}) page; the slave nodes must be started beforehand. The examples that follow continue this code in the same Python process.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.common import FileLogger
from groupdocs.search.scaling import SearchNetworkNode
from groupdocs.search.scaling.configuring import Configuration, TcpSettings

base_path = "./SearchNetwork/"
base_port = 49100

# Loading the search network configuration from a file
configuration = Configuration.load(base_path + "Configuration.xml")

# Creating, configuring and starting the master node; the timeouts are specified in milliseconds
node = SearchNetworkNode.create_master_node(
    0,
    base_path + "Node0",
    TcpSettings(base_port, 3000, 3000, 3000, 1000),
    FileLogger(base_path + "Log.txt", 4.0),
    configuration)
node.configure_all_nodes()
node.start()
```
{{< /tab >}}
{{< tab "SearchNetwork/Configuration.xml" >}}
{{< tab-text >}}
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/getting-documents-in-network/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates obtaining a list of documents indexed in the search network, as well as outputting the resulting data and the attributes of the documents to the console.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
searcher = node.searcher
indexer = node.indexer

shard_indices = node.get_shard_indices()
print()
for shard_index in shard_indices:
    infos = searcher.get_indexed_documents(shard_index)
    for info in infos:
        node_index = node.get_node_index(info.shard_index)
        print("%d: %d: %s" % (node_index, info.shard_index, info.document_info.file_path))
        attributes = indexer.get_attributes(info.document_info.file_path)
        for attribute in attributes:
            print("\t\t" + attribute)

        items = searcher.get_indexed_document_items(info)
        for item in items:
            print("\t%d: %d: %s" % (node_index, item.shard_index, item.document_info))
```
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured Python library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
