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
Configuring a search network involves creating a configuration using the [Configurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/) class and the fluent interface.

The [Configurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/) class has methods for configuring general index settings, as well as for configuring each individual network node.

The [SetIndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/setindexsettings/) method creates an instance of the [IndexSettingsConfigurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/) class, which allows you to set the following settings for each index of the distributed search network:
 - Flag for using stop words when indexing ([SetUseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/setusestopwords/) method).
 - Flag for using character replacements during indexing ([SetUseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/setusecharacterreplacements/) method).
 - Flag for using the extracted text storage, as well as the degree of text compression in the storage ([SetTextStorageSettings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/settextstoragesettings/) method).
 - Index type ([SetIndexType](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/setindextype/) method).
 - Number of threads used for searching ([SetSearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/setsearchthreads/) method).

To complete the configuration of index settings, you must call the [CompleteIndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/completeindexsettings/) method.

To configure individual search network nodes, use the [AddNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/addnode/) method, which returns an instance of the [NodeConfigurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/) class.

The [NodeConfigurator](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/) class allows you to set the following node parameters:
 - Set the required IP address and port number of the search network node ([SetTcpEndpoint](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/settcpendpoint/) method).
 - Declare the node as a receiver of log messages for all other network nodes ([AddLogSink](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/addlogsink/) method).
 - Add an indexing service, which is the input for all operations that change the contents of shards in network nodes ([AddIndexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/addindexer/) method). This service must be presented on the network exactly once.
 - Add a search service, which is the input for all search queries to the network ([AddSearcher](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/addsearcher/) method).
 - Add an extraction service used to extract data from documents added to the search network ([AddExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/addextractor/) method). Increasing the number of extraction services (one per network node) increases the performance of search network indexing.
 - Add a shard service used to store extracted data in a form suitable for searching on the network ([AddShard](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/addshard/) method). Increasing the number of shards (one for each network node) increases the performance of searching the network.

To complete the configuration of a network node, you must call the [CompleteNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/nodeconfigurator/completenode/) method.

Once the search network configuration is complete, you must call the [CompleteConfiguration](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configurator/completeconfiguration/) method, which will return an instance of the [Configuration](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configuration/) class.

An instance of the [Configuration](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configuration/) class can be saved to a file or stream in the XML format using the Save method, and subsequently can be loaded from the file or stream using the Load method.

The following code example shows how to create a search network configuration that describes four nodes.

**C#**

```csharp
string address = "127.0.0.1";
Configuration configuration = new Configurator()
    .SetIndexSettings()
        .SetUseStopWords(false)
        .SetUseCharacterReplacements(false)
        .SetTextStorageSettings(true, Compression.High)
        .SetIndexType(IndexType.NormalIndex)
        .SetSearchThreads(NumberOfThreads.Default)
        .CompleteIndexSettings()
    .AddNode(0)
        .SetTcpEndpoint(address, basePort)
        .AddLogSink()
        .AddIndexer(basePath + "Indexer0")
        .AddSearcher(basePath + "Searcher0")
        .CompleteNode()
    .AddNode(1)
        .SetTcpEndpoint(address, basePort + 1)
        .AddShard(basePath + "Shard1")
        .AddExtractor(basePath + "Extractor1")
        .CompleteNode()
    .AddNode(2)
        .SetTcpEndpoint(address, basePort + 2)
        .AddShard(basePath + "Shard2")
        .AddExtractor(basePath + "Extractor2")
        .CompleteNode()
    .AddNode(3)
        .SetTcpEndpoint(address, basePort + 3)
        .AddShard(basePath + "Shard3")
        .AddExtractor(basePath + "Extractor3")
        .CompleteNode()
    .CompleteConfiguration();
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
