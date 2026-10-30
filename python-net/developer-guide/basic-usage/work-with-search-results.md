---
id: work-with-search-results
url: search/python-net/work-with-search-results
title: Work with search results
weight: 3
description: "This article describes how to obtain information from search results and how to highlight found words and phrases in the text of documents with GroupDocs.Search for Python via .NET."
keywords: search results, found documents, found fields, occurrences, highlight search results, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
structuredData:
  showOrganization: True
  application:
    name: Document Search
    description: Index documents and search them natively with high performance using Python language and GroupDocs.Search for Python via .NET
    productCode: search
    productPlatform: python-net
  howTo:
    name: How to work with search results in Python
    description: Learn how to read search results and highlight found words in Python step by step
    steps:
      - name: Search in an index
        text: Call the search method of the Index class. The method returns a SearchResult object.
      - name: Read the found documents
        text: Call get_found_document for each document number below document_count to get a FoundDocument object with the file path, the number of occurrences and the found fields.
      - name: Highlight the search results
        text: Create a DocumentHighlighter with a FileOutputAdapter and pass it with a found document to the highlight method of the index.
---
Working with search results consists in obtaining information from objects of search results and highlighting occurrences in the text of documents.

## Obtain search result information

When a search is complete, the [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search/) method returns an object of type [SearchResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/). This page describes the information available in an object of type [SearchResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/).

From the root object of the search result, information is available on the number of documents found, the number of occurrences of the words and phrases found, as well as detailed information on each individual document. Information about an individual document found is represented by an object of the class [FoundDocument](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/).

From the object of the class [FoundDocument](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/), information is available on the full path to the document, the number of occurrences, the words and phrases found, as well as detailed information on each field of the document. Information about the document field is represented by an object of the class [FoundDocumentField](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/).

From the object of the class [FoundDocumentField](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/), information is available on the name of the document field, the number of occurrences, the words and phrases found, and the number of occurrences of each word and phrase.

Below is a code example that writes to the console the detailed information contained in an object of the class [SearchResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/) about each document, document field, word, phrase and the number of their occurrences in the document fields.

{{< alert style="info" >}}In the evaluation mode, only the first 10 found documents can be retrieved: the [get_found_document](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/get_found_document/) method raises [LicenseRestrictionException](https://reference.groupdocs.com/search/python-net/groupdocs.search.exceptions/licenserestrictionexception/) for a document number of 10 or greater. See [Licensing and evaluation]({{< ref "search/python-net/getting-started/licensing-and-subscription.md" >}}).{{< /alert >}}

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import SearchOptions, TableDiscreteFunction

# Creating an index
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Creating search options
options = SearchOptions()
options.fuzzy_search.enabled = True  # Enabling the fuzzy search
options.fuzzy_search.fuzzy_algorithm = TableDiscreteFunction(3)  # Setting the maximum number of differences to 3

# Search for documents containing the word 'Einstein' or the phrase 'Theory of Relativity'
result = index.search('Einstein OR "Theory of Relativity"', options)

# Printing the result
print("Documents:", result.document_count)
print("Total occurrences:", result.occurrence_count)
for i in range(result.document_count):
    document = result.get_found_document(i)
    print("\tDocument:", document.document_info.file_path)
    print("\tOccurrences:", document.occurrence_count)
    for field in document.found_fields:
        print("\t\tField:", field.field_name)
        print("\t\tOccurrences:", field.occurrence_count)
        # Printing found terms
        if field.terms is not None:
            for term, occurrences in zip(field.terms, field.terms_occurrences):
                print("\t\t\t" + term.ljust(20) + str(occurrences))
        # Printing found phrases
        if field.term_sequences is not None:
            for sequence, occurrences in zip(field.term_sequences, field.term_sequences_occurrences):
                print("\t\t\t" + " ".join(sequence).ljust(30) + str(occurrences))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/work-with-search-results/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/work-with-search-results/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/work-with-search-results/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/work-with-search-results/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "obtain-search-result-information.txt" >}}  
```text
Documents: 3
Total occurrences: 10
	Document: documents\einstein.txt
	Occurrences: 6
		Field: filename
		Occurrences: 1
			einstein            1
		Field: content
		Occurrences: 5
			einstein            3
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/work-with-search-results/obtain_search_result_information/obtain-search-result-information.txt)
{{< /tab >}}
{{< /tabs >}}

## Highlight search results

At the end of a search, it is usually necessary to visualize the results by highlighting the words found in the text. You can highlight search results either in the text of an entire document, or in separate segments. Detailed information about highlighting search results can be found on the page [Highlighting search results]({{< ref "search/python-net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}). Below is an example of highlighting search results in the text of an entire document.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import FileOutputAdapter
from groupdocs.search.highlighters import DocumentHighlighter
from groupdocs.search.options import OutputFormat

# Creating an index
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search for the word 'eternity'
result = index.search("eternity")

# Highlighting occurrences in text
if result.document_count > 0:
    document = result.get_found_document(0)  # Getting the first found document
    os.makedirs("./output", exist_ok=True)
    output_adapter = FileOutputAdapter(OutputFormat.HTML, "./output/Highlighted.html")  # Creating an output adapter to the file
    highlighter = DocumentHighlighter(output_adapter)  # Creating the highlighter object
    index.highlight(document, highlighter)  # Generating HTML formatted text with highlighted occurrences
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/basic-usage/work-with-search-results/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/basic-usage/work-with-search-results/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/basic-usage/work-with-search-results/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/basic-usage/work-with-search-results/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Highlighted.html" >}}  
```text
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>documents\time.txt</title>
</head>
<body>
<p>documents\time.txt</p>
<p></p>
<h3>filename</h3>
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/work-with-search-results/highlight_search_results/Highlighted.html)
{{< /tab >}}
{{< /tabs >}}

## More resources

### Advanced usage topics

To learn more about search features and get familiar how to enhance your search solution, please refer to the [advanced usage section]({{< ref "search/python-net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured Python library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
