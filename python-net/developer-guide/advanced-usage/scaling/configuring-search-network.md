---
id: configuring-search-network
url: search/python-net/configuring-search-network
title: Configuring search network
weight: 10
description: "This page contains information about configuring the search network."
keywords: configuring search network, configuring distributed index, search network configuration
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Configuring a search network involves creating a configuration using the [Configurator](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configurator/) class and its fluent interface.

The [Configurator](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configurator/) class has methods for configuring general index settings, as well as for configuring each individual network node.

## Index settings

The [set_index_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configurator/set_index_settings/) method returns an instance of the [IndexSettingsConfigurator](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/) class, which allows you to set the following settings for the index of each shard of the search network:

 - Flag for using stop words when indexing ([set_use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/set_use_stop_words/) method).
 - Flag for using character replacements during indexing ([set_use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/set_use_character_replacements/) method).
 - Flag for using the extracted text storage, as well as the degree of text compression in the storage ([set_text_storage_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/set_text_storage_settings/) method).
 - Index type ([set_index_type](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/set_index_type/) method).
 - Number of threads used for searching ([set_search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/set_search_threads/) method).

Settings that are not set have their default values. The [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) index settings cannot be configured for a search network: the index of a shard is created with their default values (both enabled). For a description of the index settings, see the [Search index settings]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}) page.

To complete the configuration of index settings, call the [complete_index_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/indexsettingsconfigurator/complete_index_settings/) method, which returns the [Configurator](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configurator/) object.

## Nodes

A search network consists of exactly one master node and one or more slave nodes. Each node is identified by a unique non-negative node index.

To configure the master node, use the [add_master_node](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configurator/add_master_node/) method, which returns an instance of the [MasterNodeConfigurator](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/masternodeconfigurator/) class. This class allows you to add the following services to the master node:

 - An indexing service, which is the input for all operations that change the contents of shards ([add_indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/masternodeconfigurator/add_indexer/) method).
 - A search service, which is the input for all search queries to the network ([add_searcher](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/masternodeconfigurator/add_searcher/) method).

The master node also receives log messages from all other nodes of the network; this is configured automatically. The master node connects to the slave nodes, so no TCP endpoint is specified for it.

To configure a slave node, use the [add_slave_node](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configurator/add_slave_node/) method, which returns an instance of the [SlaveNodeConfigurator](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/slavenodeconfigurator/) class. This class allows you to set the following node parameters:

 - The host name or IP address and the port number at which the slave node accepts connections ([set_tcp_endpoint](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/slavenodeconfigurator/set_tcp_endpoint/) method). The endpoint must be set for every slave node, and the port must be the listening port of the node (see the [Search network deployment]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/search-network-deployment.md" >}}) page).
 - An extraction service used to extract data from documents added to the search network ([add_extractor](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/slavenodeconfigurator/add_extractor/) method). Increasing the number of extraction services (one per network node) increases the performance of search network indexing.
 - A shard service used to store extracted data in a form suitable for searching ([add_shard](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/slavenodeconfigurator/add_shard/) method). Increasing the number of shards (one for each network node) increases the performance of searching the network.

Each service is given a storage path, which is the folder where the service keeps its data. A node must contain at least one service.

To complete the configuration of a node, call the [complete_node](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/nodeconfigurator/complete_node/) method of the [NodeConfigurator](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/nodeconfigurator/) class, which returns the [Configurator](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configurator/) object.

## Completing the configuration

Once the search network configuration is complete, call the [complete_configuration](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configurator/complete_configuration/) method, which returns an instance of the [Configuration](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configuration/) class. The method checks the configuration and raises `RuntimeError` if any of the following rules is violated:

 - the node indexes are unique;
 - the configuration contains exactly one master node;
 - the configuration of each node is completed;
 - the network contains exactly one indexer and exactly one searcher;
 - the network contains at least one shard.

An instance of the [Configuration](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configuration/) class can be saved to a file or a stream in the XML format using the [save](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configuration/save/) method, and subsequently loaded from the file or stream using the [load](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.configuring/configuration/load/) class method. A stream is a binary file object, such as a file opened with `open(path, "wb")` or an `io.BytesIO` object. The folder of the file must exist: the `save` method does not create it.

The following code example shows how to create a search network configuration that describes four nodes and save it to a file.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search.options import Compression, IndexType, NumberOfThreads
from groupdocs.search.scaling.configuring import Configurator

base_path = "./SearchNetwork/"
address = "127.0.0.1"
base_port = 49100

configuration = (Configurator()
    .set_index_settings()
        .set_use_stop_words(False)
        .set_use_character_replacements(False)
        .set_text_storage_settings(True, Compression.HIGH)
        .set_index_type(IndexType.NORMAL_INDEX)
        .set_search_threads(NumberOfThreads.DEFAULT)
        .complete_index_settings()
    .add_master_node(0)
        .add_indexer(base_path + "Indexer0")
        .add_searcher(base_path + "Searcher0")
        .complete_node()
    .add_slave_node(1)
        .set_tcp_endpoint(address, base_port + 1)
        .add_shard(base_path + "Shard1")
        .add_extractor(base_path + "Extractor1")
        .complete_node()
    .add_slave_node(2)
        .set_tcp_endpoint(address, base_port + 2)
        .add_shard(base_path + "Shard2")
        .add_extractor(base_path + "Extractor2")
        .complete_node()
    .add_slave_node(3)
        .set_tcp_endpoint(address, base_port + 3)
        .add_shard(base_path + "Shard3")
        .add_extractor(base_path + "Extractor3")
        .complete_node()
    .complete_configuration())

# Saving the configuration to a file; the folder must exist
os.makedirs(base_path, exist_ok=True)
configuration.save(base_path + "Configuration.xml")
```
{{< /tab >}}
{{< /tabs >}}

The following code example shows how to load the saved configuration from the file.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.scaling.configuring import Configuration

base_path = "./SearchNetwork/"

# Loading the configuration from a file
configuration = Configuration.load(base_path + "Configuration.xml")
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
