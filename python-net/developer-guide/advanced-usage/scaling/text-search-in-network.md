---
id: text-search-in-network
url: search/python-net/text-search-in-network
title: Text search in network
weight: 90
description: "This page contains information about full-text searching in the search network."
keywords: full-text search in network, text search in distributed index
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
To run a text search, use the [search_first](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/search_first/#query-options) method of the [Searcher](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/) class. This method returns the first portion of results.

To continue the search, the [search_next](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/search_next/#network_search_token) method is used, returning the second and subsequent portions of results. Continue the search while the [next_chunk_search_token](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/next_chunk_search_token/) property of the result is not *None*.

Text and object forms of the search query are supported when appropriate overloads of the search_first method are used. The options for a network search are the same as those for a single index search, but a search network performs only chunk search: set the [is_chunk_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/is_chunk_search/) option to *True*, otherwise the search_first method raises `ValueError`.

Each portion of results is returned as a [NetworkSearchResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/) object, which has the following members:

 - [document_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/document_count/) and [occurrence_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/occurrence_count/) — the number of found documents and the total number of occurrences found;
 - [get_found_document](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/get_found_document/) — returns a found document by its index; the result can also be iterated with a `for` loop;
 - [truncated](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/truncated/) and [warnings](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/warnings/) — indicate whether the result is truncated and describe the result;
 - [start_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/start_time/), [end_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/end_time/) and [search_duration](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/search_duration/) — the time of the search;
 - [node_index](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/node_index/) — the index of the node from which the result was received;
 - [next_chunk_search_token](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networksearchresult/next_chunk_search_token/) — the token for continuing the search, or *None* if the search is complete.

A found document is represented by the [NetworkFoundDocument](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkfounddocument/) class, which contains the [found_document](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkfounddocument/found_document/) object, as in a single index search, and the index of the shard where the document was found ([shard_index](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkfounddocument/shard_index/) property).

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/text-search-in-network/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates performing a full-text search on the search network.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import SearchQuery
from groupdocs.search.options import SearchOptions

word = "einstein"
use_synonym_search = True

searcher = node.searcher

query = SearchQuery.create_word_query(word)
print()
print("First time search for: " + str(query))
options = SearchOptions()
options.is_chunk_search = True
options.use_synonym_search = use_synonym_search
total_occurrences = 0
documents = []

result = searcher.search_first(query, options)

documents.extend(result)
total_occurrences += result.occurrence_count
print("Documents found (node %d): %d" % (result.node_index, result.document_count))

while result.next_chunk_search_token is not None:
    print()
    print("Next time search for: " + str(query))

    result = searcher.search_next(result.next_chunk_search_token)

    documents.extend(result)
    total_occurrences += result.occurrence_count
    print("Documents found (node %d): %d" % (result.node_index, result.document_count))

print()
print("Total occurrences: %d" % total_occurrences)
for document in documents:
    print("%d: %s (%d occurrences)" % (document.shard_index,
                                       document.found_document.document_info.file_path,
                                       document.found_document.occurrence_count))
```
{{< /tab >}}
{{< /tabs >}}

More information about search queries can be found on the [Queries in text and object form]({{< ref "search/python-net/developer-guide/advanced-usage/searching/queries-in-text-and-object-form.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured Python library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
