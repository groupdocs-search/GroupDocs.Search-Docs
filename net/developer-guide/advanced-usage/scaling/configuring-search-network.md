---
id: configuring-search-network
url: search/net/configuring-search-network
title: Configuring search network
weight: 10
description: "This page contains information about configuring the search network."
keywords: configuring search network, configuring distributed index, search network configuration
productName: GroupDocs.Search for .NET
hideChildren: False
---
Configuring a search network involves creating a configuration using the [Configurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/) class and its fluent interface.

The [Configurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/) class has methods for configuring general index settings, as well as for configuring each individual network node.

## Index settings

The [SetIndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/setindexsettings/) method returns an instance of the [IndexSettingsConfigurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/) class, which allows you to set the following settings for the index of each shard of the search network:

 - Flag for using stop words when indexing ([SetUseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/setusestopwords/) method).
 - Flag for using character replacements during indexing ([SetUseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/setusecharacterreplacements/) method).
 - Flag for using the extracted text storage, as well as the degree of text compression in the storage ([SetTextStorageSettings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/settextstoragesettings/) method).
 - Index type ([SetIndexType](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/setindextype/) method).
 - Number of threads used for searching ([SetSearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/setsearchthreads/) method).

Settings that are not set have their default values. The [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) index settings cannot be configured for a search network: the index of a shard is created with their default values (both enabled). The [UseFastLoading](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usefastloading/) setting, added in version 26.11, cannot be configured either: the index of a shard is created with its default value (*true*). For a description of the index settings, see the [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}) page.

To complete the configuration of index settings, call the [CompleteIndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/completeindexsettings/) method, which returns the [Configurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/) object.

## Nodes

A search network consists of exactly one master node and one or more slave nodes. Each node is identified by a unique non-negative node index.

To configure the master node, use the [AddMasterNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/addmasternode/) method, which returns an instance of the [MasterNodeConfigurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/masternodeconfigurator/) class. This class allows you to add the following services to the master node:

 - An indexing service, which is the input for all operations that change the contents of shards ([AddIndexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/masternodeconfigurator/addindexer/) method).
 - A search service, which is the input for all search queries to the network ([AddSearcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/masternodeconfigurator/addsearcher/) method).

The master node also receives log messages from all other nodes of the network; this is configured automatically. The master node connects to the slave nodes, so no TCP endpoint is specified for it.

To configure a slave node, use the [AddSlaveNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/addslavenode/) method, which returns an instance of the [SlaveNodeConfigurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/slavenodeconfigurator/) class. This class allows you to set the following node parameters:

 - The host name or IP address and the port number at which the slave node accepts connections ([SetTcpEndpoint](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/slavenodeconfigurator/settcpendpoint/) method). The endpoint must be set for every slave node, and the port must be the listening port of the node (see the [Search network deployment]({{< ref "search/net/developer-guide/advanced-usage/scaling/search-network-deployment.md" >}}) page).
 - An extraction service used to extract data from documents added to the search network ([AddExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/slavenodeconfigurator/addextractor/) method). Increasing the number of extraction services (one per network node) increases the performance of search network indexing.
 - A shard service used to store extracted data in a form suitable for searching ([AddShard](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/slavenodeconfigurator/addshard/) method). Increasing the number of shards (one for each network node) increases the performance of searching the network.

Each service is given a storage path, which is the folder where the service keeps its data. A node must contain at least one service.

To complete the configuration of a node, call the [CompleteNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/completenode/) method of the [NodeConfigurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/) class, which returns the [Configurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/) object.

## Completing the configuration

Once the search network configuration is complete, call the [CompleteConfiguration](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/completeconfiguration/) method, which returns an instance of the [Configuration](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configuration/) class. The method checks the configuration and throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if any of the following rules is violated:

 - the node indexes are unique;
 - the configuration contains exactly one master node;
 - the configuration of each node is completed;
 - the network contains exactly one indexer and exactly one searcher;
 - the network contains at least one shard.

An instance of the [Configuration](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configuration/) class can be saved to a file or a stream in the XML format using the [Save](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configuration/save/) method, and subsequently loaded from the file or stream using the static [Load](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configuration/load/) method. Since version 26.11, the [Load](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configuration/load/) method refuses a configuration with a document type definition (a DOCTYPE) with [XmlException](https://learn.microsoft.com/dotnet/api/system.xml.xmlexception) and reads nothing outside the file or the stream. A configuration saved by the [Save](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configuration/save/) method has no document type definition; remove the DOCTYPE from a configuration written by hand.

The following code example shows how to create a search network configuration that describes four nodes and save it to a file.

**C#**

```csharp
string basePath = @"c:\SearchNetwork\";
string address = "127.0.0.1";
int basePort = 49100;

Configuration configuration = new Configurator()
    .SetIndexSettings()
        .SetUseStopWords(false)
        .SetUseCharacterReplacements(false)
        .SetTextStorageSettings(true, Compression.High)
        .SetIndexType(IndexType.NormalIndex)
        .SetSearchThreads(NumberOfThreads.Default)
        .CompleteIndexSettings()
    .AddMasterNode(0)
        .AddIndexer(basePath + "Indexer0")
        .AddSearcher(basePath + "Searcher0")
        .CompleteNode()
    .AddSlaveNode(1)
        .SetTcpEndpoint(address, basePort + 1)
        .AddShard(basePath + "Shard1")
        .AddExtractor(basePath + "Extractor1")
        .CompleteNode()
    .AddSlaveNode(2)
        .SetTcpEndpoint(address, basePort + 2)
        .AddShard(basePath + "Shard2")
        .AddExtractor(basePath + "Extractor2")
        .CompleteNode()
    .AddSlaveNode(3)
        .SetTcpEndpoint(address, basePort + 3)
        .AddShard(basePath + "Shard3")
        .AddExtractor(basePath + "Extractor3")
        .CompleteNode()
    .CompleteConfiguration();

// Saving the configuration to a file
configuration.Save(basePath + "Configuration.xml");
```

The following code example shows how to load the saved configuration from the file.

**C#**

```csharp
string basePath = @"c:\SearchNetwork\";

// Loading the configuration from a file
Configuration configuration = Configuration.Load(basePath + "Configuration.xml");
```

## Network settings

The network settings of a node are not a part of the configuration: they are passed to the node when it is created (see the [Search network deployment]({{< ref "search/net/developer-guide/advanced-usage/scaling/search-network-deployment.md" >}}) page). An instance of the [TcpSettings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/) class specifies the port at which a slave node accepts connections, as well as the send, receive, connect and retry timeouts in milliseconds. Since version 26.11, the [connect timeout](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/connecttimeout/) also limits the time in which a node waits for each message of the exchange that starts a connection with another node.

Since version 26.11, the [ResponseTimeout](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/responsetimeout/) property of the settings of the master node limits the time in milliseconds in which the master node waits for the answer of a node to a request. A request that is not answered in time fails: a call of the search network throws [SearchException](https://reference.groupdocs.com/search/net/groupdocs.search.exceptions/searchexception/), and an operation reports the document or the shard by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/erroroccurred/) event. The default value is *Timeout.Infinite*: the master node waits without a limit. A value should be larger than the longest request of a node; for example, the optimization of a large shard may take minutes. Regardless of this setting, the requests of a node fail at once when the node is lost (see [Errors and lost nodes]({{< ref "search/net/developer-guide/advanced-usage/scaling/search-network-deployment.md#errors-and-lost-nodes" >}})).

## Authentication of nodes

Since version 26.11, the nodes of a search network can prove to each other that they belong to the network. A node without a network key cannot tell the nodes of the network from other computers: a slave node carries out the commands of any computer that connects to its port, for example to index or delete documents, to read their texts or to replace dictionaries. Such a node writes a warning to its log (by the Trace method of the logger) when it is created.

 - The [NetworkKey](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/networkkey/) property sets the secret key of the network, which must have at least 16 characters and be the same on all nodes; a shorter key throws [ArgumentException](https://learn.microsoft.com/dotnet/api/system.argumentexception). When two nodes connect, each of them proves that it knows the key, and every message is signed: a computer without the key can neither give commands to a node nor answer for a node, and a changed, repeated or forged message breaks the connection. The key is not a part of the configuration and is not sent over the network.
 - The [AllowedMasterAddresses](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/allowedmasteraddresses/) property of the settings of a slave node sets the addresses (IPv4 addresses or host names) from which the slave node accepts the connection of the master node; a connection from another address is closed at once and logged. The master node does not use this property.

Nodes with different keys, or a node with a key and a node without one, do not connect, and both nodes write the reason to their logs.

{{< alert style="info" >}}The messages of the nodes are not encrypted: for example, the texts of documents and the passwords of documents are sent as they are. Keep the nodes in a trusted network, or connect them through an encrypted tunnel.{{< /alert >}}

The following code example shows how to create a slave node that accepts the connection of the master node only from the address of the master server, and the master node with the same network key. The master node waits for the answer of a node no longer than 10 minutes.

**C#**

```csharp
string basePath = @"c:\SearchNetwork\";
int basePort = 49100;
string masterAddress = "10.0.0.5"; // The address of the master server

// The same secret key on every node of the network, for example from the protected settings of the application
string networkKey = Environment.GetEnvironmentVariable("SEARCH_NETWORK_KEY");

// Creating and starting the slave node with index 1 on a slave server; the timeouts are specified in milliseconds
TcpSettings slaveSettings = new TcpSettings(basePort + 1, 3000, 3000, 3000, 1000);
slaveSettings.NetworkKey = networkKey;
slaveSettings.AllowedMasterAddresses = new string[] { masterAddress }; // The master node is accepted only from this address
SearchNetworkNode node1 = SearchNetworkNode.CreateSlaveNode(1, basePath + "Node1", slaveSettings);
node1.Start();

// Creating the master node with index 0 and the same key on the master server
Configuration configuration = Configuration.Load(basePath + "Configuration.xml");
TcpSettings masterSettings = new TcpSettings(basePort, 3000, 3000, 3000, 1000);
masterSettings.NetworkKey = networkKey;
masterSettings.ResponseTimeout = 600000; // The master node waits for the answer of a node no longer than 10 minutes
SearchNetworkNode node0 = SearchNetworkNode.CreateMasterNode(
    0,
    basePath + "Node0",
    masterSettings,
    new FileLogger(basePath + "Log.txt", 4.0),
    configuration);
node0.ConfigureAllNodes();
node0.Start();
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
