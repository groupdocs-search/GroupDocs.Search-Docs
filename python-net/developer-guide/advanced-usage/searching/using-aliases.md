---
id: using-aliases
url: search/python-net/using-aliases
title: Using aliases
weight: 28
description: "This article describes the use of aliases, which allows you to reduce the length of search queries in text form."
keywords: Using aliases
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The use of aliases allows you to reduce the length of search queries in text form. To use an alias in a search query, you must enter the @ symbol with the name of the alias after it without spaces.

Before a search query in text form is parsed, each alias in the query is replaced with the text associated with it in the alias dictionary (see [Search flow]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-flow.md" >}})). The following rules apply:

*   An alias name can contain only characters from the ranges a-z and 0-9; an attempt to add an alias with other characters throws ArgumentException (ValueError in Python). Alias names are stored in lower case, so the name used in a query must be written in lower case.
*   An alias is replaced only if it is separated from the adjacent text by spaces or parentheses, or is located at the beginning or end of the query. For example, in the query "@t OR (@e)", both aliases are replaced.
*   Aliases are not used in search queries in object form.

It is recommended to enclose the text of an alias in parentheses if it contains operators, so that the substituted text is combined with the rest of the query as a single subquery, as in the example below.

Aliases are stored in the alias dictionary. By default, the alias dictionary is empty. The alias dictionary can be changed at any time. Detailed information on managing the alias dictionary can be found on the [Alias dictionary]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/alias-dictionary.md" >}}) page of the [Managing dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

An example of using aliases for searching is presented below.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Adding aliases to the alias dictionary
index.dictionaries.alias_dictionary.add("t", "(theory OR relativity)")
index.dictionaries.alias_dictionary.add("e", "(Einstein OR Albert)")

# Search in the index
result = index.search("@t OR @e")

# Printing the words found in each document
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/using-aliases/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/using-aliases/documents/relativity-primer.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/using-aliases/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/using-aliases/documents/nobel-prize.txt), [documents/coal-mining.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/using-aliases/documents/coal-mining.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "using-aliases.txt" >}}  
```text
Documents found: 4
    einstein-biography.txt ['einstein', 'theory', 'relativity', 'albert']
    nobel-prize.txt ['einstein']
    quantum-theory.txt ['theory']
    relativity-primer.txt ['relativity', 'theory']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/using-aliases/using_aliases/using-aliases.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
