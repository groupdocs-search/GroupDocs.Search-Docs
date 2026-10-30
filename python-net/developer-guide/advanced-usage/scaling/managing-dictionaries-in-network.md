---
id: managing-dictionaries-in-network
url: search/python-net/managing-dictionaries-in-network
title: Managing dictionaries in network
weight: 140
description: "This page contains information about managing dictionaries of shards in the search network."
keywords: managing dictionaries in search network, managing dictionaries in distributed index, working with dictionaries in search network, working with dictionaries in distributed index
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
To obtain a dictionary from a shard of the search network, use the [get_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_dictionary/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class. The first parameter of the [get_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_dictionary/) method is the type of the required dictionary, and the second parameter is the index of the shard in the network. The method returns an object of type [DictionaryBase](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/); in Python, the object already has the class of the required dictionary, for example [SynonymDictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/synonymdictionary/) or [Alphabet](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/alphabet/), so no cast is needed.

The [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class also has methods that return a dictionary of the required type: [get_alphabet](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_alphabet/), [get_tokenization_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_tokenization_dictionary/), [get_character_replacement_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_character_replacement_dictionary/), [get_stop_word_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_stop_word_dictionary/), [get_synonym_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_synonym_dictionary/), [get_homophone_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_homophone_dictionary/), [get_alias_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_alias_dictionary/), [get_spelling_corrector](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_spelling_corrector/) and [get_password_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_password_dictionary/).

The returned object is a copy of the dictionary of the shard. You can perform all operations on it that are valid for a dictionary in a single index; the changes do not affect the shard until the dictionary is set back. For information about working with dictionaries, see the [Managing dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

After setting up the dictionary, return it to the shard using the [set_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/set_dictionary/#dictionary-shard_index) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class, specifying the index of the desired shard. Alternatively, you can set the dictionary to all shards by calling the [set_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/set_dictionary/#dictionary) method with one parameter, without specifying the shard index. The [set_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/set_dictionary/) method replaces the whole content of the dictionary of a shard with the content of the passed dictionary.

{{< alert style="info" >}}The alphabet, the tokenization dictionary, the stop word dictionary and the character replacement dictionary define how the text of documents is divided into terms. Therefore, a shard accepts another content of such a dictionary only while its index contains no documents and is not performing an operation. A shard that does not accept the dictionary does not raise an exception: it keeps its own dictionary and reports the reason as an error to the log of the search network and by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/error_occurred/) event of its node. Configure these dictionaries before adding the first documents to the search network. The other dictionaries can be changed at any time. See also the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/managing-dictionaries-in-network/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates adding a group of synonyms to the synonym dictionary of all shards.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
group = ["achieve", "accomplish", "attain", "reach"]
clear_before_adding = False  # Specifies whether to remove the existing synonyms

print("Adding synonyms")

indexer = node.indexer

# Getting the synonym dictionary of the first shard
indices = node.get_shard_indices()
dictionary = indexer.get_synonym_dictionary(indices[0])

if clear_before_adding:
    dictionary.clear()

# Adding the group of synonyms
dictionary.add_range([group])

# Setting the dictionary to all shards
indexer.set_dictionary(dictionary)
```
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the code that creates the master node, demonstrates setting the tokenization dictionary of all shards before adding documents to the search network.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.dictionaries import CharacterType

indexer = node.indexer

# Getting the tokenization dictionary of the first shard
indices = node.get_shard_indices()
tokenization_dictionary = indexer.get_tokenization_dictionary(indices[0])

# Setting the Letter type for Hiragana and Katakana characters
code_points = list(range(0x3040, 0x30FF + 1))
tokenization_dictionary.set_range(code_points, CharacterType.LETTER)

# Setting the dictionary to all shards; a shard whose index contains documents keeps its own dictionary
indexer.set_dictionary(tokenization_dictionary)
```
{{< /tab >}}
{{< /tabs >}}

For more information on the tokenization dictionary, see the [Tokenization dictionary]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured Python library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
