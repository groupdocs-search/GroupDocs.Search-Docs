---
id: queries-in-text-and-object-form
url: search/python-net/queries-in-text-and-object-form
title: Queries in text and object form
weight: 15
description: "This article describes two ways to create a search query: in text form and in object form."
keywords: Queries in text and object form
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
There are two ways to create a search query: in text or object form.

A search query in text form is simpler but less flexible. The flexibility of queries in object form lies in the fact that it is possible to set separate search options for each subquery. In object form, you can also use, for example, a regular expression query as a part of a more complex one. All combinations of nesting search queries in object form can be found on the page [Nesting search queries in object form]({{< ref "search/python-net/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form.md" >}}).

Each search query in text form is parsed and transformed into a query in object form before the search. Therefore, a query in object form saves the parsing step. A syntax error in a text query does not throw an exception: the search returns an empty result, and the error message is passed to the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index.

The full specification of the query language in text form is presented on the page [Query language specification]({{< ref "search/python-net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}). Syntax with examples of all elements allowed in text search queries is presented on the page [Search operation table]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-operation-table.md" >}}).

Each example in this documentation is usually given with the search query in text and object form.

An example of a complex query in object form is given below.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery
from groupdocs.search.options import SearchOptions, TableDiscreteFunction

# Creating index
index = Index("./index")

# Indexing
index.add("./documents")

# Creating subquery 1 - simple word query
subquery1 = SearchQuery.create_word_query("future")
subquery1.search_options = SearchOptions()  # Setting search options only for subquery 1
subquery1.search_options.fuzzy_search.enabled = True
subquery1.search_options.fuzzy_search.fuzzy_algorithm = TableDiscreteFunction(3)  # The maximum number of differences is 3

# Creating subquery 2 - numeric range query
subquery2 = SearchQuery.create_numeric_range_query(1, 1000000)

# Creating subquery 3 - regular expression query
subquery3 = SearchQuery.create_regex_query(r"(.)\1")

# Combining subqueries into one query - phrase search query
query = SearchQuery.create_phrase_search_query(subquery1, subquery2, subquery3)

# Creating overall search options that limit the number of occurrences returned
options = SearchOptions()
options.max_occurrence_count_per_term = 1000000
options.max_total_occurrence_count = 10000000

# Searching
result = index.search(query, options)

# The result may contain the following word sequences:
# future 12 balloons
# picture 3 balloons

# Printing the word sequences found
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path))
    for field in document.found_fields:
        for sequence in field.term_sequences:
            print("       ", " ".join(sequence))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/library-stats.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/queries-in-text-and-object-form/documents/library-stats.txt), [documents/launch-plan.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/queries-in-text-and-object-form/documents/launch-plan.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "queries-in-text-and-object-form.txt" >}}  
```text
Documents found: 1
    launch-plan.txt
        future 12 balloons
        picture 3 balloons
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/queries-in-text-and-object-form/queries_in_text_and_object_form/queries-in-text-and-object-form.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
