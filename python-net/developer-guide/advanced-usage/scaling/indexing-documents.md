---
id: indexing-documents
url: search/python-net/indexing-documents
title: Indexing documents
weight: 40
description: "This page contains information about indexing documents in the search network."
keywords: search network indexing, distributed index indexing, adding documents in search network
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Documents can be added to the search network using the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/add/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class. The indexer is available through the [indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/indexer/) property of the master node.

The first parameter of the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/add/) method is a list of documents to be added. Only documents created from a stream or from a structure are supported. The key of a document identifies it in the search network; it is used, for example, to delete the document or change its attributes.

The second parameter of the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/add/) method is a list of passwords for opening the added documents. The list must have the same length as the list of documents; use *None* for a document without a password.

The third parameter of the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/add/) method is the indexing options object. By default, the method returns when indexing is complete. If the [is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/is_async/) option is set to *True*, the method returns immediately, and the completion of indexing can be tracked by subscribing to the [indexing_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/indexing_completed/) event. Text recognition in images requires an OCR connector set on the nodes with extraction services by the [set_ocr_connector](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searchnetworknode/set_ocr_connector/) method.

The current state of the search network can be obtained with the [status](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/status/) property of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class.

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates adding documents from streams.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import datetime
import os

from groupdocs.search.common import Document
from groupdocs.search.options import IndexingOptions

folder = "./documents"
file_paths = [os.path.join(folder, name) for name in os.listdir(folder)
              if os.path.isfile(os.path.join(folder, name))]

streams = []
documents = []
passwords = []
for file_path in file_paths:
    modification_date = datetime.datetime.fromtimestamp(os.path.getmtime(file_path))
    file_name = os.path.basename(file_path)  # The file name is used as the document key
    extension = os.path.splitext(file_path)[1]
    stream = open(file_path, "rb")
    streams.append(stream)
    document = Document.create_from_stream(
        file_name,
        modification_date,
        extension,
        stream)
    documents.append(document)
    passwords.append(None)  # The documents are not protected with passwords

indexer = node.indexer

options = IndexingOptions()
options.use_raw_text_extraction = False
options.image_indexing_options.enabled_for_separate_images = True
options.image_indexing_options.enabled_for_embedded_images = True
options.image_indexing_options.enabled_for_container_item_images = True

# Indexing the documents; the method returns when indexing is complete
indexer.add(documents, passwords, options)

for stream in streams:
    stream.close()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-life.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/einstein-life.txt), [documents/archive-catalogue.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/archive-catalogue.pdf), [documents/relativity-lecture.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/relativity-lecture.docx), [documents/besso-letters.zip](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/besso-letters.zip), [documents/Image.png](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/Image.png)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Extracting data separately from indexing

The [extract](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/extract/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class only extracts data from documents using the extraction services of the network, without indexing. The data extracted from each document are passed to the [data_extracted](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/data_extracted/) event, and the completion of the operation is reported by the [extraction_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/extraction_completed/) event. The [is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/is_async/) option has the same meaning as for the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/add/) method.

The extracted data can then be indexed with the overload of the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/add/#data-options) method that accepts a list of [ExtractedData](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extracteddata/) objects. This overload does not use the extraction services.

The following code example, which continues the code that creates the master node, demonstrates extracting data from documents and indexing the extracted data. The [data_extracted](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.events/nodeeventhub/data_extracted/) event is raised on engine worker threads, so the handler protects the list with a lock.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import datetime
import os
import threading

from groupdocs.search.common import Document
from groupdocs.search.options import IndexingOptions

folder = "./documents"
file_paths = [os.path.join(folder, name) for name in os.listdir(folder)
              if os.path.isfile(os.path.join(folder, name))]

streams = []
documents = []
passwords = []
for file_path in file_paths:
    stream = open(file_path, "rb")
    streams.append(stream)
    documents.append(Document.create_from_stream(
        os.path.basename(file_path),
        datetime.datetime.fromtimestamp(os.path.getmtime(file_path)),
        os.path.splitext(file_path)[1],
        stream))
    passwords.append(None)

# Collecting the extracted data
extracted_data = []
lock = threading.Lock()


def on_data_extracted(sender, args):
    with lock:
        extracted_data.append(args.data)


node.events.data_extracted += on_data_extracted
node.events.extraction_completed += lambda sender, args: print("Extraction complete")

indexer = node.indexer

# Extracting data from the documents; the method returns when extraction is complete
indexer.extract(documents, passwords, IndexingOptions())

for stream in streams:
    stream.close()

# Indexing the extracted data
indexer.add(extracted_data, IndexingOptions())
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-life.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/einstein-life.txt), [documents/archive-catalogue.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/archive-catalogue.pdf), [documents/relativity-lecture.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/relativity-lecture.docx), [documents/besso-letters.zip](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/besso-letters.zip), [documents/Image.png](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/indexing-documents/documents/Image.png)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

## Deleting all data

To delete all indexed data from all shards of the search network, use the [delete_all_data](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/delete_all_data/) method of the [Indexer](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/) class. The method returns when all shards have deleted their data.

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
# Deleting all indexed data from the search network (continues the code that creates the master node)
node.indexer.delete_all_data()
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
