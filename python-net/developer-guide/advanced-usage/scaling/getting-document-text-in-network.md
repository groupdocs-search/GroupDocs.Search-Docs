---
id: getting-document-text-in-network
url: search/python-net/getting-document-text-in-network
title: Getting document text in network
weight: 130
description: "This page contains information about getting document text in the search network."
keywords: getting document text in network, getting document text in distributed index, getting document text in search network, getting document content in search network
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
To obtain the extracted text of indexed documents in the search network, the [get_document_text](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/get_document_text/) method of the [Searcher](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/) class is used.

The first parameter of the [get_document_text](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/get_document_text/) method is a document represented by the [NetworkDocumentInfo](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkdocumentinfo/) class. A list of all indexed documents in the search network can be obtained using the [get_indexed_documents](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/get_indexed_documents/) method of the [Searcher](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/) class.

The second parameter of the [get_document_text](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/get_document_text/) method is the result output adapter. In this operation, the same output adapter classes are used as those used in similar operations of a single index. For information about output adapters, see the [Output adapters]({{< ref "search/python-net/developer-guide/advanced-usage/searching/output-adapters.md" >}}) page. The text is available only if the text storage is enabled in the [configuration]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/configuring-search-network.md" >}}) of the search network; otherwise, the output adapter receives no text, and the log of the search network reports the error.

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/getting-document-text-in-network/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates retrieving the text of a document indexed in the search network, as well as outputting the resulting data to the console.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.common import StringOutputAdapter
from groupdocs.search.options import OutputFormat

file_name = "archive-catalogue.pdf"  # The name of the document to get the text of

searcher = node.searcher

documents = []
shard_indices = node.get_shard_indices()
for shard_index in shard_indices:
    infos = searcher.get_indexed_documents(shard_index)
    documents.extend(infos)
    for info in infos:
        items = searcher.get_indexed_document_items(info)
        documents.extend(items)

for document in documents:
    if file_name in str(document.document_info):
        print()
        print(document.document_info)

        output_adapter = StringOutputAdapter(OutputFormat.PLAIN_TEXT)
        searcher.get_document_text(document, output_adapter)

        print(output_adapter.get_result())
        break
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
