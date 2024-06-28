---
id: configuring-search-network
url: search/nodejs-java/configuring-search-network
title: Configuring search network
weight: 10
description: "This page contains information about configuring the search network."
keywords: configuring search network, configuring distributed index, search network configuration
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Configuring a search network involves creating a configuration using the [Configurator](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/configurator/) class and the fluent interface.

The [Configurator](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/configurator/) class has methods for configuring general index settings, as well as for configuring each individual network node.

The [setIndexSettings](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/configurator/#setIndexSettings--) method creates an instance of the [IndexSettingsConfigurator](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/indexsettingsconfigurator/) class, which allows you to set the following settings for each index of the distributed search network:
 - Flag for using stop words when indexing ([setUseStopWords](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/indexsettingsconfigurator/#setUseStopWords-boolean-) method).
 - Flag for using character replacements during indexing ([setUseCharacterReplacements](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/indexsettingsconfigurator/#setUseCharacterReplacements-boolean-) method).
 - Flag for using the extracted text storage, as well as the degree of text compression in the storage ([setTextStorageSettings](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/indexsettingsconfigurator/#setTextStorageSettings-boolean-com.groupdocs.search.options.Compression-) method).
 - Index type ([setIndexType](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/indexsettingsconfigurator/#setIndexType-com.groupdocs.search.options.IndexType-) method).
 - Number of threads used for searching ([setSearchThreads](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/indexsettingsconfigurator/#setSearchThreads-com.groupdocs.search.options.NumberOfThreads-) method).

To complete the configuration of index settings, you must call the [completeIndexSettings](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/indexsettingsconfigurator/#completeIndexSettings--) method.

To configure individual search network nodes, use the [addNode](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/configurator/#addNode-int-) method, which returns an instance of the [NodeConfigurator](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/nodeconfigurator/) class.

The [NodeConfigurator](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/nodeconfigurator/) class allows you to set the following node parameters:
 - Set the required IP address and port number of the search network node ([setTcpEndpoint](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/nodeconfigurator/#setTcpEndpoint-java.lang.String-int-) method).
 - Declare the node as a receiver of log messages for all other network nodes ([addLogSink](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/nodeconfigurator/#addLogSink--) method).
 - Add an indexing service, which is the input for all operations that change the contents of shards in network nodes ([addIndexer](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/nodeconfigurator/#addIndexer-java.lang.String-) method). This service must be presented on the network exactly once.
 - Add a search service, which is the input for all search queries to the network ([addSearcher](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/nodeconfigurator/#addSearcher-java.lang.String-) method).
 - Add an extraction service used to extract data from documents added to the search network ([addExtractor](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/nodeconfigurator/#addExtractor-java.lang.String-) method). Increasing the number of extraction services (one per network node) increases the performance of search network indexing.
 - Add a shard service used to store extracted data in a form suitable for searching on the network ([addShard](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/nodeconfigurator/#addShard-java.lang.String-) method). Increasing the number of shards (one for each network node) increases the performance of searching the network.

To complete the configuration of a network node, you must call the [completeNode](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/nodeconfigurator/#completeNode--) method.

Once the search network configuration is complete, you must call the [completeConfiguration](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/configurator/#completeConfiguration--) method, which will return an instance of the [Configuration](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/configuration/) class.

An instance of the [Configuration](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.scaling.configuring/configuration/) class can be saved to a file or stream in the XML format using the Save method, and subsequently can be loaded from the file or stream using the Load method.

The following code example shows how to create a search network configuration that describes four nodes.

```javascript
String address = "127.0.0.1";
Configuration configuration = new Configurator()
    .setIndexSettings()
        .setUseStopWords(false)
        .setUseCharacterReplacements(false)
        .setTextStorageSettings(true, Compression.High)
        .setIndexType(IndexType.NormalIndex)
        .setSearchThreads(NumberOfThreads.Default)
        .completeIndexSettings()
    .addNode(0)
        .setTcpEndpoint(address, basePort)
        .addLogSink()
        .addIndexer(basePath + "Indexer0")
        .addSearcher(basePath + "Searcher0")
        .completeNode()
    .addNode(1)
        .setTcpEndpoint(address, basePort + 1)
        .addShard(basePath + "Shard1")
        .addExtractor(basePath + "Extractor1")
        .completeNode()
    .addNode(2)
        .setTcpEndpoint(address, basePort + 2)
        .addShard(basePath + "Shard2")
        .addExtractor(basePath + "Extractor2")
        .completeNode()
    .addNode(3)
        .setTcpEndpoint(address, basePort + 3)
        .addShard(basePath + "Shard3")
        .addExtractor(basePath + "Extractor3")
        .completeNode()
    .completeConfiguration();
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
