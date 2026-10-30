---
id: optimizing-shards
url: search/python-net/optimizing-shards
title: Optimizing shards
weight: 60
description: "This page contains information about optimizing shards in the search network."
keywords: optimize search network, optimize distributed index, search network optimization, distributed index optimization, shard optimization
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
All search network shards can be optimized with a single call of the [optimize](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/optimize/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class.

The optimization merges the index segments of each shard, minimizing their number, and physically removes the data of deleted documents. As a result, search performance increases and the shards take less disk space. After optimizing a very large index, a shard may still contain several segments. The progress of the optimization can be tracked by subscribing to the [optimization_progress_changed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/optimization_progress_changed/) event.

An asynchronous execution flag is available as an option ([OptimizeOptions.is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/optimizeoptions/is_async/)). In the case of asynchronous optimization, you can track the completion of the operation by subscribing to the [optimization_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/optimization_completed/) event.

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/optimizing-shards/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates how to perform search network optimization.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.options import OptimizeOptions

print("Optimizing shards")
indexer = node.indexer
options = OptimizeOptions()
indexer.optimize(options)
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
