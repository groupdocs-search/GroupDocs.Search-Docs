---
id: synchronizing-shards
url: search/python-net/synchronizing-shards
title: Synchronizing shards
weight: 70
description: "This page contains information about synchronizing shards in the search network."
keywords: synchronize search network, synchronize distributed index, search network synchronization, distributed index synchronization, shard synchronization
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
If there is any communication interruption in the search network, some operations may not complete correctly. To resolve such problems, call the [synchronize](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/synchronize/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class. The method synchronizes the list of indexed documents kept by the indexer with the documents in the shards.

The asynchronous operation flag is available as an option ([SynchronizeOptions.is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/synchronizeoptions/is_async/)). You can track the completion of the operation by subscribing to the [synchronization_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/synchronization_completed/) event.

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/synchronizing-shards/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates how to synchronize search network nodes.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.options import SynchronizeOptions

print("Synchronizing shards")
indexer = node.indexer
options = SynchronizeOptions()
indexer.synchronize(options)
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
