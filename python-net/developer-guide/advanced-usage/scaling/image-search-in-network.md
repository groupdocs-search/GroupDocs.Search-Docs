---
id: image-search-in-network
url: search/python-net/image-search-in-network
title: Image search in network
weight: 100
description: "This page contains information about reverse image search in the search network."
keywords: image search in network, image search in distributed index, reverse image search in network
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
To run a reverse image search, use the [search_first](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/search_first/#image-options) method of the [Searcher](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/) class. This method returns the first portion of results as a [NetworkImageSearchResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkimagesearchresult/) object, which contains the images found in one shard.

To continue the search, the [search_next](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/search_next/#network_image_search_token) method is used, returning the second and subsequent portions of results. Continue the search while the [network_image_search_token](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkimagesearchresult/network_image_search_token/) property of the result is not *None*.

The search image object and search options object are the same as those used for single index searches. Images can be found only if image indexing was enabled when the documents were [added]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/indexing-documents.md" >}}).

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/image-search-in-network/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates performing a reverse image search on the search network.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.common import SearchImage
from groupdocs.search.options import ImageSearchOptions

# Creating a search image from a file
search_image = SearchImage.create("./documents/Image.png")
hash_differences = 8  # The maximum allowed difference between the image hashes

print("First image search")

searcher = node.searcher
options = ImageSearchOptions()
options.hash_differences = hash_differences
total = 0

result = searcher.search_first(search_image, options)
print("Images found (shard %d): %d" % (result.shard_index, result.image_count))
total += result.image_count

while result.network_image_search_token is not None:
    print()
    print("Next image search")

    result = searcher.search_next(result.network_image_search_token)
    print("Images found (shard %d): %d" % (result.shard_index, result.image_count))
    total += result.image_count

print()
print("Total images found (diffs = %d): %d" % (hash_differences, total))
```
{{< /tab >}}
{{< tab "documents/Image.png" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents/Image.png](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/image-search-in-network/documents/Image.png)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

More information about reverse image search can be found on the [Reverse image search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/reverse-image-search.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured Python library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
