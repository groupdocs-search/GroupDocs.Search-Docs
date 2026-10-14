---
id: search-network-deployment
url: search/net/search-network-deployment
title: Search network deployment
weight: 20
description: "This page contains information about deployment of the search network."
keywords: search network deployment, deployment of search network, distributed index deployment, deployment of distributed index
productName: GroupDocs.Search for .NET
hideChildren: False
---
The GroupDocs.Search search network consists of one master node and several slave nodes. Each node is a server with the .NET platform deployed on it. On each server, an application is deployed that creates an instance of the [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) class.

An instance of the [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) class provides the services described in the [configuration]({{< ref "search/net/developer-guide/advanced-usage/scaling/configuring-search-network.md" >}}) for this node:
 - Indexing service (master node);
 - Search service (master node);
 - Extraction services (slave nodes);
 - Shard services (slave nodes).

The indexing and search services also provide an interface to access all search network features through the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/indexer/) and [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/searcher/) properties of the master node.

## Creating nodes

The [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/) class has no public constructor. Its instances are created with the following static methods:

 - [CreateMasterNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/createmasternode/)(int nodeIndex, string storagePath, INetworkSettings networkSettings, ILogger logger, Configuration configuration) creates the master node. The configuration is required; it describes all nodes of the search network.
 - [CreateSlaveNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/createslavenode/)(int nodeIndex, string storagePath, INetworkSettings networkSettings) and CreateSlaveNode(int nodeIndex, string storagePath, INetworkSettings networkSettings, ILogger logger) create a slave node. A slave node receives its configuration from the master node and saves it in its storage folder, so after a restart, the node uses the saved configuration.

The parameters of these methods are the following:

 - *nodeIndex* — the index of the node, which must match the node index in the configuration;
 - *storagePath* — the folder where the node keeps its data;
 - *networkSettings* — an instance of the [TcpSettings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/) class, which specifies the port at which a slave node accepts connections (it must match the port specified for the node in the configuration), as well as the send, receive, connect and retry timeouts in milliseconds;
 - *logger* — an implementation of the [ILogger](https://reference.groupdocs.com/search/net/groupdocs.search.common/ilogger/) interface, for example, [FileLogger](https://reference.groupdocs.com/search/net/groupdocs.search.common/filelogger/), or *null*. The logger of the master node receives the log messages of all nodes of the search network.

A node can also be given an OCR connector ([SetOcrConnector](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/setocrconnector/) method), which is used by its extraction services, a word forms provider ([SetWordFormsProvider](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/setwordformsprovider/) method), which is used by its shards, and custom text extractors ([CustomExtractors](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/customextractors/) property).

## Starting the search network

To deploy the GroupDocs.Search search network, perform the following steps:
 - On each slave server, create a slave node with the [CreateSlaveNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/createslavenode/) method and call its [Start](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/start/) method.
 - On the master server, create the master node with the [CreateMasterNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/createmasternode/) method, passing the configuration object to it.
 - Call the [ConfigureAllNodes](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/configureallnodes/) method of the master node. The method sends the configuration to all slave nodes and waits until they are configured. To configure only one slave node, for example, a node that has been replaced, use the [ConfigureNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/configurenode/) method. Only the master node can configure other nodes.
 - Call the [Start](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/start/) method of the master node.

To stop a node, call its [Dispose](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/dispose/) method.

All nodes of a search network must use the same version of GroupDocs.Search. For upgrading a search network to version 26.10, see the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

The following code example demonstrates starting a slave node on a server.

**C#**

```csharp
string basePath = @"c:\SearchNetwork\";
int basePort = 49100;

// The timeouts are specified in milliseconds
int sendTimeout = 3000;
int receiveTimeout = 3000;
int connectTimeout = 3000;
int retryTimeout = 1000;

// Creating and starting the slave node with index 1, which listens on the port specified for it in the configuration
SearchNetworkNode node1 = SearchNetworkNode.CreateSlaveNode(
    1,
    basePath + "Node1",
    new TcpSettings(basePort + 1, sendTimeout, receiveTimeout, connectTimeout, retryTimeout));
node1.Start();
```

The following code example demonstrates starting the master node on a server. The configuration is created as shown on the [Configuring search network]({{< ref "search/net/developer-guide/advanced-usage/scaling/configuring-search-network.md" >}}) page and loaded from a file.

**C#**

```csharp
string basePath = @"c:\SearchNetwork\";
int basePort = 49100;

// The timeouts are specified in milliseconds
int sendTimeout = 3000;
int receiveTimeout = 3000;
int connectTimeout = 3000;
int retryTimeout = 1000;

// Loading the search network configuration from a file
Configuration configuration = Configuration.Load(basePath + "Configuration.xml");

// Creating the master node with index 0
SearchNetworkNode node0 = SearchNetworkNode.CreateMasterNode(
    0,
    basePath + "Node0",
    new TcpSettings(basePort, sendTimeout, receiveTimeout, connectTimeout, retryTimeout),
    new ConsoleLogger(),
    configuration);

node0.Events.ConfigurationCompleted += (s, e) =>
{
    Console.WriteLine("Configuration complete");
};

Console.WriteLine();
Console.WriteLine("Configuring the search network");
node0.ConfigureAllNodes();

Console.WriteLine("Launching the search network");
node0.Start();
```

The following code example demonstrates the implementation of a simple console logger.

**C#**

```csharp
class ConsoleLogger : ILogger
{
    public void Error(string message)
    {
        Console.WriteLine("Error: " + message);
    }

    public void Trace(string message)
    {
        Console.WriteLine("Trace: " + message);
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
