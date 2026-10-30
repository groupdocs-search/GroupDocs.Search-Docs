---
id: search-results
url: search/python-net/search-results
title: Search results
weight: 24
description: "This article describes the classes that represent search results and shows how to work with them."
keywords: Search results
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Search results are represented by the [SearchResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/) class, an instance of which is returned by the [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/search/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class. The [search](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexrepository/search/) method of the [IndexRepository](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexrepository/) class also returns an instance of the [SearchResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/) class.

The [SearchResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/) class contains the following members:

*   The [document_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/document_count/) property returns the number of documents found.
*   The [occurrence_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/occurrence_count/) property returns the total number of occurrences found.
*   The [truncated](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/truncated/) property returns a value indicating that the result is truncated due to limits specified in the search options.
*   The [warnings](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/warnings/) property returns warnings describing the result, for example, a warning about the presence of a stop word in a search query.
*   The [next_chunk_search_token](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/next_chunk_search_token/) property returns a chunk search token to search for the next chunk, or None if there are no more chunks. For details on search by chunks, see the [Search by chunks]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) page.
*   The [start_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/start_time/) property returns the start time of the search.
*   The [end_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/end_time/) property returns the end time of the search.
*   The [search_duration](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/search_duration/) property returns the search duration.
*   The [get_found_document](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/get_found_document/) method returns the found document by index.
*   The [get_enumerator](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/get_enumerator/) method returns an enumerator that iterates through the collection of the documents found; in Python, iterate over the result itself, for example, with `for document in result:`.

The found document is represented by an instance of a [FoundDocument](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/) class. The [FoundDocument](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/) class contains the following members:

*   The [document_info](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/document_info/) property returns the document info object. Its members are described below.
*   The [relevance](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/relevance/) property returns the relevance of the document in the search result.
*   The [occurrence_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/occurrence_count/) property returns the number of occurrences found in the document.
*   The [found_fields](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/found_fields/) property returns the found document fields.
*   The [terms](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/terms/) property returns the found terms. The value is evaluated each time the property is accessed based on the data for each document field found.
*   The [term_sequences](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/term_sequences/) property returns the found term sequences. A stop word inside a found phrase appears as `*`: the phrase `"theory of special relativity"` gives the sequence `theory * special relativity`.
*   The [attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/attributes/) property returns the attributes associated with the found document. The attributes are retrieved only if the [retrieve_attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/retrieve_attributes/) search option is enabled; otherwise, the property returns an empty list. For details, see the [Search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-options.md" >}}) page.
*   The [serialize](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/serialize/) method serializes the current found document instance to bytes.
*   The [deserialize](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocument/deserialize/) method deserializes an instance of found document from bytes.

The indexed document is described by an instance of the [DocumentInfo](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/) class, which contains the following members:

*   The [file_path](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/file_path/) property returns the file path for a document indexed from a file, or the document key for a document indexed from a stream or a structure.
*   The [file_type](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/file_type/) property returns the file type of the document.
*   The [format_family](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/format_family/) property returns the format family of the document.
*   The [inner_path](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/inner_path/) property returns the inner path of an item of a container document, for example, an item of a ZIP archive or an attachment of an email message.
*   The [inner_path_parts](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/inner_path_parts/) property returns the parts of the inner path of an item of a container document.
*   The [document_source_kind](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/document_source_kind/) property returns the kind of the document source: a file, a stream, a structure, previously extracted data, or a temporary file.
*   The [modification_time](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/modification_time/) property returns the modification time of the document at the time of indexing; for a document indexed from a file, this is the last write time of the file.
*   The [indexed_with_error](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/indexed_with_error/) property returns a value indicating whether an error occurred while indexing the document, for example, if the indexing of the document was interrupted by cancellation. Such a document indexed from a file is indexed again by the next [update](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/update/) operation; a document indexed from a stream, a structure or extracted data has to be added again.
*   The [serialize](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/serialize/) and [deserialize](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/deserialize/) methods serialize the document info to bytes and deserialize it from bytes.

The found document field is represented by an instance of a [FoundDocumentField](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/) class. The [FoundDocumentField](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/) class contains the following members:

*   The [field_name](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/field_name/) property returns the field name.
*   The [occurrence_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/occurrence_count/) property returns the number of occurrences found.
*   The [terms](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/terms/) property returns the terms found.
*   The [terms_occurrences](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/terms_occurrences/) property returns the occurrences of the found terms.
*   The [term_sequences](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/term_sequences/) property returns the term sequences found, with a stop word inside a phrase shown as `*`, as for the found document.
*   The [term_sequences_occurrences](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/term_sequences_occurrences/) property returns the occurrences of the found term sequences.
*   The [serialize](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/serialize/) method serializes the current found document field instance to bytes.
*   The [deserialize](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/founddocumentfield/deserialize/) method deserializes an instance of found document field from bytes.

The found documents come in the order in which they were indexed, and [add](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/add/) indexes the files of a folder in the order in which the file system lists them: name order on Windows, but not on Linux. [get_indexed_documents](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_indexed_documents/) keeps the same order. So on Linux, an example on these pages can list its documents in another order than its output, which comes from Windows. To list found documents in the same order on every system, sort them, for example by relevance: `sorted(result, key=lambda found: found.relevance, reverse=True)`.

Since version 26.10, search uses as many threads as there are processor cores by default. Therefore, the order of the terms found by fuzzy, wildcard, regular expression, numeric range search and spelling correction may differ from run to run. To get the same order every time, set the [search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/search_threads/) index setting to NumberOfThreads.ONE. For details, see the [Search index settings]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md#search_threads-property" >}}) page.

The following example shows how to print information on the documents found in the console.

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
print("Documents: " + str(result.document_count))
print("Total occurrences: " + str(result.occurrence_count))
for i in range(result.document_count):
    document = result.get_found_document(i)
    print("\tDocument: " + document.document_info.file_path)
    print("\tOccurrences: " + str(document.occurrence_count))
    for field in document.found_fields:
        print("\t\tField: " + field.field_name)
        print("\t\tOccurrences: " + str(field.occurrence_count))
        # Printing found terms
        if field.terms is not None:
            for k in range(len(field.terms)):
                print("\t\t\t" + field.terms[k].ljust(20) + str(field.terms_occurrences[k]))
        # Printing found phrases
        if field.term_sequences is not None:
            for k in range(len(field.term_sequences)):
                sequence = " ".join(field.term_sequences[k])
                print("\t\t\t" + sequence.ljust(30) + str(field.term_sequences_occurrences[k]))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-results/documents/einstein-biography.txt), [documents/relativity-primer.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-results/documents/relativity-primer.txt), [documents/newspaper-archive.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/search-results/documents/newspaper-archive.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-results.txt" >}}  
```text
Documents: 3
Total occurrences: 9
	Document: documents\einstein-biography.txt
	Occurrences: 5
		Field: filename
		Occurrences: 1
			einstein            1
		Field: content
		Occurrences: 4
			einstein            2
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/search-results/search_results/search-results.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
