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
 - *networkSettings* — an instance of the [TcpSettings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/) class, which specifies the port at which a slave node accepts connections (it must match the port specified for the node in the configuration), as well as the send, receive, connect and retry timeouts in milliseconds; since version 26.11, it also sets the network key of the node, the response timeout of the master node and the addresses from which a slave node accepts the master node (see the [Network settings]({{< ref "search/net/developer-guide/advanced-usage/scaling/configuring-search-network.md#network-settings" >}}) and [Authentication of nodes]({{< ref "search/net/developer-guide/advanced-usage/scaling/configuring-search-network.md#authentication-of-nodes" >}}) sections);
 - *logger* — an implementation of the [ILogger](https://reference.groupdocs.com/search/net/groupdocs.search.common/ilogger/) interface, for example, [FileLogger](https://reference.groupdocs.com/search/net/groupdocs.search.common/filelogger/), or *null*. The logger of the master node receives the log messages of all nodes of the search network.

A node can also be given an OCR connector ([SetOcrConnector](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/setocrconnector/) method), which is used by its extraction services, a word forms provider ([SetWordFormsProvider](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/setwordformsprovider/) method), which is used by its shards, and custom text extractors ([CustomExtractors](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/customextractors/) property).

## Starting the search network

To deploy the GroupDocs.Search search network, perform the following steps:
 - On each slave server, create a slave node with the [CreateSlaveNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/createslavenode/) method and call its [Start](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/start/) method.
 - On the master server, create the master node with the [CreateMasterNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/createmasternode/) method, passing the configuration object to it.
 - Call the [ConfigureAllNodes](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/configureallnodes/) method of the master node. The method sends the configuration to all slave nodes and waits until they are configured. To configure only one slave node, for example, a node that has been replaced, use the [ConfigureNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/configurenode/) method. Only the master node can configure other nodes. Since version 26.11, these methods throw [SearchException](https://reference.groupdocs.com/search/net/groupdocs.search.exceptions/searchexception/) that names the node and the error when a slave node does not accept the configuration, for example a node created with a node index that the configuration does not have, or when the connection with a node breaks.
 - Call the [Start](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/start/) method of the master node.

To stop a node, call its [Dispose](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/dispose/) method (see [Stopping a node](#stopping-a-node)).

All nodes of a search network must use the same version of GroupDocs.Search. Since version 26.11, a node does not connect to a node of an earlier version and writes the reason to its log, so all nodes have to be upgraded at once. For upgrading a search network to version 26.10, see the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

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

## Errors and lost nodes

Since version 26.11, a node answers a request that it cannot carry out with the error, and the master node stops waiting for a node that is lost:

 - A call that waits for the answers of the nodes throws [SearchException](https://reference.groupdocs.com/search/net/groupdocs.search.exceptions/searchexception/) with the error of the node: the searches, the highlighting, the document text and the lists of indexed documents of the [Searcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/), the [GetAttributes](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getattributes/), [GetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getdictionary/), [SetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/setdictionary/) and [DeleteAllData](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/deletealldata/) methods of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/), and the ConfigureAllNodes and ConfigureNode methods. Catch SearchException around these calls. GetAttributes of a document that the network has not indexed throws [ArgumentException](https://learn.microsoft.com/dotnet/api/system.argumentexception) without asking a shard (see [Working with attributes]({{< ref "search/net/developer-guide/advanced-usage/scaling/working-with-attributes.md" >}})).
 - An operation of the indexer (adding, extraction, deletion, optimization, changing attributes and synchronization) completes with the other documents and shards: a document or a shard that a node could not process is reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/erroroccurred/) event of the master node with the indexes of the node and the service. The errors that the index of a shard or of an extractor reports while it carries out an operation of the indexer, a highlighting or the getting of a document text, for example for a document whose text cannot be extracted, are reported by this event as well.
 - A slave node sends a short message to the master node every 5 seconds. The master node considers a node lost when it sends nothing for 15 seconds, when its connection is broken or when it has been started again. The calls that wait for the answers of a lost node throw SearchException at once, and a call to a lost node throws SearchException without sending its request. An operation of the indexer sends the documents of a lost node to the other nodes. A document that only the lost node can process, for example a new version of a document indexed in a shard of the lost node, and a shard of the lost node in an optimization, a synchronization, a change of attributes or a deletion are reported by the ErrorOccurred event, and the operation completes; repeat the operation for them when the node is connected again. The master node connects to a lost node again and uses it again after its next answer.
 - The response timeout of the master node limits the time in which it waits for the answer of a node to a request; by default, the master node waits without a limit (see [Network settings]({{< ref "search/net/developer-guide/advanced-usage/scaling/configuring-search-network.md#network-settings" >}})).
 - The adding and the extraction of documents, the text search and the highlighting stop when the cancellation of their options is requested (see the [Indexing documents]({{< ref "search/net/developer-guide/advanced-usage/scaling/indexing-documents.md" >}}), [Text search in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/text-search-in-network.md" >}}) and [Highlighting results in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/highlighting-results-in-network.md" >}}) pages).

## Stopping a node

Since version 26.11, the [Dispose](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/dispose/) method stops the operations of the node: a call that waits for the search network, for example a synchronous indexing or a search, throws [ObjectDisposedException](https://learn.microsoft.com/dotnet/api/system.objectdisposedexception), an asynchronous operation ends without its completion event, the operations of the shards and the extraction of the node are cancelled, and the requests that wait in the queues of its shards and extractors are dropped. The [Start](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/start/), [ConfigureAllNodes](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/configureallnodes/) and [ConfigureNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/configurenode/) methods of a disposed node and the methods of its indexer and searcher throw ObjectDisposedException.

The Dispose method waits until the work of the node ends; a custom text extractor or an OCR connector that is running is waited for until it returns. No event of the node occurs afterwards. When the method returns, the node writes nothing more to its folders, so they can be deleted. A handler of an event of the node, or of an index, that disposes the node does not wait: the work of the node may still be ending when Dispose returns.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
