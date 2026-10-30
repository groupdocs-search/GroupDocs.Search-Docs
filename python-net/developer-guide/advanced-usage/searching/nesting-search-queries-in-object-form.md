---
id: nesting-search-queries-in-object-form
url: search/python-net/nesting-search-queries-in-object-form
title: Nesting search queries in object form
weight: 11
description: "This article describes which search queries in object form can be nested in each other."
keywords: Nesting search queries in object form
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The table below shows all combinations of nesting search queries in object form with an indication of their possibility (+) or impossibility (–).

<table>
    <thead>
        <tr>
            <th>Nested query type</th>
            <th colspan=5>Parent query type</th>
        </tr>
        <tr>
            <th width="20%"></th>
            <th width="16%">Field</th>
            <th width="16%">PhraseSearch</th>
            <th width="16%">Not</th>
            <th width="16%">Or</th>
            <th width="16%">And</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Word</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>WordPattern</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>NumericRange</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Regex</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Field</td>
            <td>+</td>
            <td>–</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>DateRange</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>PhraseSearch</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Not</td>
            <td>+</td>
            <td>–</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Or</td>
            <td>+</td>
            <td>–</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>And</td>
            <td>+</td>
            <td>–</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Wildcard</td>
            <td>–</td>
            <td>+</td>
            <td>–</td>
            <td>–</td>
            <td>–</td>
        </tr>
    </tbody>
</table>

An attempt to create an impossible combination throws ArgumentException (ValueError in Python) with the message "Query of type '...' can not be a child of '...'.". A wildcard query created with the [create_wildcard_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_wildcard_query/) method can be used only as a child of a phrase search query; passing it to the [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search/) method directly throws ArgumentException (ValueError in Python).

The [create_phrase_search_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_phrase_search_query/), [create_and_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_and_query/) and [create_or_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_or_query/) methods take the child queries as separate arguments or as one list, which must contain at least two queries. The [create_and_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_and_query/) and [create_or_query](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/create_or_query/) methods also have overloads with two child queries.

A created query can be examined with the following members of the [SearchQuery](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/) class:

*   The [child_count](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/child_count/) property returns the number of child queries.
*   The [get_child](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/get_child/) method returns a child query by its index; the [first_child](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/first_child/) and [second_child](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/second_child/) properties return the first and the second child queries.
*   The [field_name](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/field_name/) property returns the name of the field to search in, specified by a field query that contains this query, or None.
*   The [search_options](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/search_options/) property gets or sets the search options of this query.
*   The [to_string](https://reference.groupdocs.com/search/python-net/groupdocs.search/searchquery/to_string/) method returns the query in text form.

The following example demonstrates the creation of a nested query and the traversal of the query tree.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, SearchQuery
from groupdocs.search.options import CommonFieldNames

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Creating a query that searches for the phrase 'theory of * relativity' in the content field
phrase_query = SearchQuery.create_phrase_search_query(
    SearchQuery.create_word_query("theory"),
    SearchQuery.create_word_query("of"),
    SearchQuery.create_wildcard_query(0, 1),
    SearchQuery.create_word_query("relativity"))
field_query = SearchQuery.create_field_query(CommonFieldNames.CONTENT, phrase_query)

# Printing the query tree
print(field_query.to_string())
for i in range(phrase_query.child_count):
    child = phrase_query.get_child(i)
    print("\t" + child.to_string() + " (field: " + str(child.field_name) + ")")

# Searching in the index
result = index.search(field_query)

# Printing the documents and the phrases found
# (stop words and the words matched by a wildcard are reported as *)
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
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form/documents/relativity-primer.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form/documents/quantum-theory.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "nesting-search-queries-in-object-form.txt" >}}  
```text
content:"theory of *0~~1 relativity"
	theory (field: content)
	of (field: content)
	*0~~1 (field: content)
	relativity (field: content)
Documents found: 2
    einstein-biography.txt
        theory * relativity
    relativity-primer.txt
        theory * relativity
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form/nesting_search_queries_in_object_form/nesting-search-queries-in-object-form.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
