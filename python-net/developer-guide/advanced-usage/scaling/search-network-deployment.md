---
id: search-network-deployment
url: search/python-net/search-network-deployment
title: Search network deployment
weight: 20
description: "This page contains information about deployment of the search network."
keywords: search network deployment, deployment of search network, distributed index deployment, deployment of distributed index
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The GroupDocs.Search search network consists of one master node and several slave nodes. Each node is a server with Python and GroupDocs.Search for Python via .NET installed on it (see [Installation]({{< ref "search/python-net/getting-started/installation.md" >}}); the package contains the .NET runtime it needs). On each server, a Python script runs that creates an instance of the [SearchNetworkNode](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/) class. A node works while the process that created it runs. To try a search network on one computer, run the nodes in separate processes on the same computer, as the examples in this section do.

An instance of the [SearchNetworkNode](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/) class provides the services described in the [configuration]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/configuring-search-network.md" >}}) for this node:
 - Indexing service (master node);
 - Search service (master node);
 - Extraction services (slave nodes);
 - Shard services (slave nodes).

The indexing and search services also provide an interface to access all search network features through the [indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/indexer/) and [searcher](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/searcher/) properties of the master node.

## Creating nodes

The [SearchNetworkNode](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/) class has no public constructor. Its instances are created with the following class methods:

 - [create_master_node](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/create_master_node/)(node_index, storage_path, network_settings, logger, configuration) creates the master node. The configuration is required; it describes all nodes of the search network.
 - [create_slave_node](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/create_slave_node/)(node_index, storage_path, network_settings) and create_slave_node(node_index, storage_path, network_settings, logger) create a slave node. A slave node receives its configuration from the master node and saves it in its storage folder, so after a restart, the node uses the saved configuration.

The parameters of these methods are the following:

 - *node_index* — the index of the node, which must match the node index in the configuration;
 - *storage_path* — the folder where the node keeps its data;
 - *network_settings* — an instance of the [TcpSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/tcpsettings/) class, which specifies the port at which a slave node accepts connections (it must match the port specified for the node in the configuration), as well as the send, receive, connect and retry timeouts in milliseconds;
 - *logger* — an implementation of the [ILogger](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/ilogger/) interface, for example, [FileLogger](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/filelogger/), or *None*. In Python, the logger can also be an object of your own class with the `error(message)` and `trace(message)` methods, or a function that takes the level (`"error"` or `"trace"`) and the message. The logger of the master node receives the log messages of all nodes of the search network.

A node can also be given an OCR connector ([set_ocr_connector](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/set_ocr_connector/) method), which is used by its extraction services, a word forms provider ([set_word_forms_provider](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/set_word_forms_provider/) method), which is used by its shards, and custom text extractors ([custom_extractors](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/custom_extractors/) property).

## Starting the search network

To deploy the GroupDocs.Search search network, perform the following steps:
 - On each slave server, create a slave node with the [create_slave_node](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/create_slave_node/) method and call its [start](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/start/) method.
 - On the master server, create the master node with the [create_master_node](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/create_master_node/) method, passing the configuration object to it.
 - Call the [configure_all_nodes](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/configure_all_nodes/) method of the master node. The method sends the configuration to all slave nodes and waits until they are configured. To configure only one slave node, for example, a node that has been replaced, use the [configure_node](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/configure_node/) method. Only the master node can configure other nodes.
 - Call the [start](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/start/) method of the master node.

To stop a node, call its [dispose](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/dispose/) method.

{{< alert style="info" >}}The [configure_all_nodes](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/configure_all_nodes/) method waits until every slave node of the configuration accepts the connection; it does not time out, and the log of the master node reports a connection error at each attempt. Start the slave nodes first. A slave node listens on all network interfaces at the port of its [TcpSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/tcpsettings/). On Windows, a port in a range reserved by the system (run `netsh interface ipv4 show excludedportrange protocol=tcp`; Hyper-V, WSL and Docker reserve such ranges) cannot be used: the node does not listen, and only its logger reports `Establishing connection error`.

In version 26.10, the slave nodes may refuse a new master node after the previous master node has stopped, whether by its `dispose` method or because its process ended: the new master node then waits in `configure_all_nodes` while its log reports connection errors. Therefore, create the master node once and run the examples of this section in that process, for example in an interactive Python session: first the code that creates the master node, then the examples one after another. Restart the slave nodes before you start a new master node.{{< /alert >}}

All nodes of a search network must use the same version of GroupDocs.Search. For upgrading a search network to version 26.10, see the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

The following code example demonstrates starting slave nodes on a server. The script starts the slave nodes whose indexes are passed on the command line, for example `python slave_nodes.py 1` on the server of node 1. Without arguments, it starts all three slave nodes of the configuration created on the [Configuring search network]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/configuring-search-network.md" >}}) page, which lets you try the search network on one computer. The nodes work until you press Enter.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import sys

from groupdocs.search.scaling import SearchNetworkNode
from groupdocs.search.scaling.configuring import TcpSettings

base_path = "./SearchNetwork/"
base_port = 49100

# The timeouts are specified in milliseconds
send_timeout = 3000
receive_timeout = 3000
connect_timeout = 3000
retry_timeout = 1000

# The indexes of the slave nodes to start; all slave nodes of the configuration by default
node_indexes = [int(arg) for arg in sys.argv[1:]] or [1, 2, 3]

nodes = []
for node_index in node_indexes:
    # Creating and starting the slave node, which listens on the port specified for it in the configuration
    node = SearchNetworkNode.create_slave_node(
        node_index,
        base_path + "Node" + str(node_index),
        TcpSettings(base_port + node_index, send_timeout, receive_timeout, connect_timeout, retry_timeout))
    node.start()
    nodes.append(node)

# The nodes work while the script runs
input("Press Enter to stop the slave nodes\n")
for node in nodes:
    node.dispose()
```
{{< /tab >}}
{{< /tabs >}}

The following code example demonstrates starting the master node on a server. The configuration is created as shown on the [Configuring search network]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/configuring-search-network.md" >}}) page and loaded from a file. The logger class is shown in the next example; save it as `console_logger.py` next to this script.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.scaling import SearchNetworkNode
from groupdocs.search.scaling.configuring import Configuration, TcpSettings

from console_logger import ConsoleLogger

base_path = "./SearchNetwork/"
base_port = 49100

# The timeouts are specified in milliseconds
send_timeout = 3000
receive_timeout = 3000
connect_timeout = 3000
retry_timeout = 1000

# Loading the search network configuration from a file
configuration = Configuration.load(base_path + "Configuration.xml")

# Creating the master node with index 0
node0 = SearchNetworkNode.create_master_node(
    0,
    base_path + "Node0",
    TcpSettings(base_port, send_timeout, receive_timeout, connect_timeout, retry_timeout),
    ConsoleLogger(),
    configuration)


def on_configuration_completed(sender, args):
    print("Configuration complete")


node0.events.configuration_completed += on_configuration_completed

print()
print("Configuring the search network")
node0.configure_all_nodes()

print("Launching the search network")
node0.start()

# The master node works while the script runs
input("Press Enter to stop the master node\n")
```
{{< /tab >}}
{{< tab "SearchNetwork/Configuration.xml" >}}
{{< tab-text >}}
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/search-network-deployment/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example demonstrates the implementation of a simple console logger.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
class ConsoleLogger:
    def error(self, message):
        print("Error: " + message)

    def trace(self, message):
        print("Trace: " + message)
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
