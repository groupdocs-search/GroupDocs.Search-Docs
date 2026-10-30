---
id: deleting-documents
url: search/python-net/deleting-documents
title: Deleting documents
weight: 50
description: "This page contains information about deleting documents in the search network."
keywords: search network deleting documents, distributed index removing documents, delete documents in search network
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Documents can be deleted from the search network using the [delete](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/delete/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class.

To delete documents, pass a list of document keys, as well as a delete options object, to the [delete](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/delete/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class. The keys are the keys that were specified when the documents were [added]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/indexing-documents.md" >}}).

Deleted documents are excluded from search results immediately, but their data are physically removed from the shards only by [optimization]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/optimizing-shards.md" >}}).

An asynchronous delete flag is available as an option ([DeleteOptions.is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/deleteoptions/is_async/)). In the case of asynchronous deletion, you can track the completion of the operation by subscribing to the [deletion_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/deletion_completed/) event.

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/deleting-documents/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates how to delete documents from the search network. The documents were added from the files of the specified folder with the file names as document keys.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search.options import DeleteOptions

print()
print("Deleting documents")

folder = "./documents"
file_names = [name for name in os.listdir(folder) if os.path.isfile(os.path.join(folder, name))]

indexer = node.indexer

options = DeleteOptions()
indexer.delete(file_names, options)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-life.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/deleting-documents/documents/einstein-life.txt), [documents/archive-catalogue.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/deleting-documents/documents/archive-catalogue.pdf), [documents/relativity-lecture.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/deleting-documents/documents/relativity-lecture.docx), [documents/besso-letters.zip](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/deleting-documents/documents/besso-letters.zip), [documents/Image.png](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/deleting-documents/documents/Image.png)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured Python library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
