---
id: search-network-node-events
url: search/python-net/search-network-node-events
title: Search network node events
weight: 30
description: "This page contains information about the purpose and use of all search network events."
keywords: search network events, network events, subscribe to event, track progress
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains information about the purpose and use of all search network events. The events are available through the [events](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/events/) property of the [SearchNetworkNode](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/) class, which returns an instance of the [NodeEventHub](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/) class.

A handler is a function, a lambda or another callable that takes two arguments: the sender and the event arguments. Subscribe a handler with `+=`, for example `node.events.indexing_completed += on_indexing_completed`; `-=` removes the subscription, and `clear()` removes all the handlers of the event, for example `node.events.indexing_completed.clear()`. Most node events are raised on engine worker threads, several of them at the same time, so a handler that collects data must protect it, for example with a `threading.Lock`.

## configuration_completed event

The [configuration_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/configuration_completed/) event occurs when the search network configuration process is finished. It is raised on an engine worker thread and can arrive just after [configure_all_nodes](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/configure_all_nodes/) returns, so a script that ends right after that call may not see it.

## indexing_completed event

The [indexing_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/indexing_completed/) event occurs when indexing of all enqueued documents is finished.

## extraction_completed event

The [extraction_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/extraction_completed/) event occurs when the extraction of all enqueued documents started by the [extract](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/extract/) method is finished. See the [Indexing documents]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/indexing-documents.md" >}}) page.

## deletion_completed event

The [deletion_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/deletion_completed/) event occurs when all enqueued deletions of documents are finished.

## optimization_completed event

The [optimization_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/optimization_completed/) event occurs when optimization of all nodes is finished.

## synchronization_completed event

The [synchronization_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/synchronization_completed/) event occurs when synchronization with all nodes is finished.

## attribute_changes_completed event

The [attribute_changes_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/attribute_changes_completed/) event occurs when all enqueued attribute changes are finished.

## status_changed event

The [status_changed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/status_changed/) event occurs when the search network status changes.

## data_extracted event

The [data_extracted](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/data_extracted/) event occurs when the data have been extracted from a document. The extracted data are available through the [data](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/dataextractedeventargs/data/) property of the event arguments.

## document_indexed event

The [document_indexed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/document_indexed/) event occurs when a document has been indexed.

## document_deleted event

The [document_deleted](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/document_deleted/) event occurs when a document has been deleted.

## error_occurred event

The [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/error_occurred/) event of a node occurs when an error occurs on this node. The event arguments contain the indexes of the node and the service where the error occurred. The errors of all nodes are also written to the logger of the master node.

## indexing_progress_changed event

The [indexing_progress_changed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/indexing_progress_changed/) event occurs when the progress of the indexing operation has changed.

## optimization_progress_changed event

The [optimization_progress_changed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/optimization_progress_changed/) event occurs when the progress of the optimization operation has changed.

## Code example

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/search-network-node-events/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates subscribing to the search network events. The handlers print their messages when the operations described on the next pages of this section run, for example, indexing, deleting or optimizing.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
node.events.indexing_completed += lambda sender, args: print("Indexing complete")
node.events.extraction_completed += lambda sender, args: print("Extraction complete")
node.events.deletion_completed += lambda sender, args: print("Deleting complete")
node.events.optimization_completed += lambda sender, args: print("Optimization complete")
node.events.synchronization_completed += lambda sender, args: print("Synchronization complete")
node.events.attribute_changes_completed += lambda sender, args: print("Attribute changes complete")
node.events.status_changed += lambda sender, args: print(
    "Status changed: %s -> %s" % (args.old_status.name, args.new_status.name))
node.events.data_extracted += lambda sender, args: print(
    "Data extracted (%d): %s" % (args.extractor_index, args.document_key))
node.events.document_indexed += lambda sender, args: print(
    "Document indexed (%d): %s" % (args.shard_index, args.document_key))
node.events.document_deleted += lambda sender, args: print(
    "Document deleted (%d): %s" % (args.shard_index, args.document_key))
node.events.error_occurred += lambda sender, args: print(
    "Error occurred (%d, %d): %s" % (args.node_index, args.service_index, args.message))
node.events.indexing_progress_changed += lambda sender, args: print(
    "Indexing progress changed (%d, %d): %s" % (args.node_index, args.service_index, args.progress_percentage))
node.events.optimization_progress_changed += lambda sender, args: print(
    "Optimization progress changed (%d, %d): %s" % (args.node_index, args.service_index, args.progress_percentage))
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
