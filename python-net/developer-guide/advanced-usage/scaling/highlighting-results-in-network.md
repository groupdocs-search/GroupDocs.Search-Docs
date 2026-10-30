---
id: highlighting-results-in-network
url: search/python-net/highlighting-results-in-network
title: Highlighting results in network
weight: 110
description: "This page contains information about highlighting search results in the search network."
keywords: highlighting search results in network, highlighting results in distributed index, highlighting results in search network
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Search results in the search network can be highlighted in the text of the found document using the [highlight](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/highlight/) method of the [Searcher](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/searcher/) class. The found document is passed to the method as a [NetworkFoundDocument](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling.results/networkfounddocument/) object obtained from a [search result]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/text-search-in-network.md" >}}).

The set of available classes for highlighting text, as well as the highlighting options, are identical to those used for highlighting search results in a single index. Please see also the [Highlighting search results]({{< ref "search/python-net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}) page. Highlighting needs the extracted text of the document, so the text storage must be enabled in the [configuration]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/configuring-search-network.md" >}}) of the search network; otherwise, the highlighter returns no fragments, and the log of the search network reports the error.

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
The following sample file is used in this example: [SearchNetwork/Configuration.xml](/search/python-net/_sample_files/developer-guide/advanced-usage/scaling/highlighting-results-in-network/SearchNetwork/Configuration.xml)
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}

The following code example, which continues the previous one, demonstrates highlighting search results on the search network. A search network performs only chunk search, so the example sets the [is_chunk_search](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/is_chunk_search/) option and takes the first found document from the first portion of results that contains documents.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.highlighters import FragmentHighlighter
from groupdocs.search.options import HighlightOptions, OutputFormat, SearchOptions

query = "Einstein"
max_fragments = 5  # The maximum number of fragments to output for each field

searcher = node.searcher

# Searching in the search network
search_options = SearchOptions()
search_options.is_chunk_search = True
search_result = searcher.search_first(query, search_options)
while search_result.document_count == 0 and search_result.next_chunk_search_token is not None:
    search_result = searcher.search_next(search_result.next_chunk_search_token)
if search_result.document_count > 0:
    # Getting the first found document
    document = search_result.get_found_document(0)

    highlighter = FragmentHighlighter(OutputFormat.PLAIN_TEXT)

    options = HighlightOptions()
    options.terms_after = 5
    options.terms_before = 5
    options.terms_total = 15
    searcher.highlight(document, highlighter, options)

    result = highlighter.get_result()
    print()
    for container in result:
        if container.count == 0:
            continue

        fragments = container.get_fragments()
        print(container.field_name)
        count = min(len(fragments), max_fragments)
        for j in range(count):
            print("\t" + fragments[j])
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
