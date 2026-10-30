---
id: working-with-attributes
url: search/python-net/working-with-attributes
title: Working with attributes
weight: 80
description: "This page contains information about working with attributes in the search network."
keywords: managing attributes, attribute management, setting attributes, document attributes in search network
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Text attributes of documents are changed without the need to re-index documents in the search network.

To change document attributes, use the [change_attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/change_attributes/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class. The changes are described by an [AttributeChangeBatch](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/attributechangebatch/) object, in which documents are specified by their keys. The attributes are passed to the methods of the batch as a list of strings.

The asynchronous operation flag is available as an option ([ChangeAttributesOptions.is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/changeattributesoptions/is_async/)). You can track the completion of the operation by subscribing to the [attribute_changes_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/attribute_changes_completed/) event.

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/working-with-attributes/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates changing document attributes on the search network.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.common import AttributeChangeBatch
from groupdocs.search.options import ChangeAttributesOptions

document_key = "archive-catalogue.pdf"  # The key of an indexed document
attribute = "important"

print("Adding attribute: " + attribute)

indexer = node.indexer

batch = AttributeChangeBatch()
batch.add(document_key, [attribute])
options = ChangeAttributesOptions()
indexer.change_attributes(batch, options)
```
{{< /tab >}}
{{< /tabs >}}

To get all the attributes of a document, use the [get_attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/get_attributes/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class. You must pass the document key as a parameter.

The following code example, which continues the code that creates the master node, demonstrates retrieving document attributes from the search network.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
searcher = node.searcher
indexer = node.indexer

shard_indices = node.get_shard_indices()
print()
for shard_index in shard_indices:
    infos = searcher.get_indexed_documents(shard_index)
    for info in infos:
        node_index = node.get_node_index(info.shard_index)
        print("%d: %d: %s: %s" % (node_index, info.shard_index,
                                  info.document_info.indexed_with_error, info.document_info.file_path))
        attributes = indexer.get_attributes(info.document_info.file_path)
        for attribute in attributes:
            print("\t\t" + attribute)
```
{{< /tab >}}
{{< /tabs >}}

More information about working with text attributes of documents can be found on the [Document attributes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured Python library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
