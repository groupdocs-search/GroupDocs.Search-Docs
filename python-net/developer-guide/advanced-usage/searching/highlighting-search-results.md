---
id: highlighting-search-results
url: search/python-net/highlighting-search-results
title: Highlighting search results
weight: 8
description: "This article describes how to highlight search results in the text of a document."
keywords: highlight search results
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page describes how to highlight search results in the text of a document.

## Hit highlighting in the text of entire document

After performing a search, occurrences of found words and phrases for a particular document can be highlighted in the text of the document using the [highlight](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/highlight/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class. To do this, a highlighter of the corresponding type must be passed as an argument to the method.

The Index class also provides an overload of the [highlight](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/highlight/) method, which takes an object of the [HighlightOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/) class as an argument. The [HighlightOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/) class contains properties for setting the following options:

*   [custom_extractor](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textoptions/custom_extractor/) is an extractor used during indexing, it is necessary if the text of the document was not saved in the index.
*   [additional_fields](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textoptions/additional_fields/) are additional document fields added during document indexing which are also necessary if the document text was not saved in the index.
*   [cancellation](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textoptions/cancellation/) is an object used to cancel the operation.
*   [generate_head](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/textoptions/generate_head/) is a flag to specify whether the Head tag is generated in the output HTML. The default value is True.
*   [metadata_indexing_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/metadata_indexing_options/) is an object for specifying metadata indexing options. This property and the other properties inherited from the [BaseOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/) class (for example, [encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/encoding/), [auto_detect_encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/auto_detect_encoding/) and [ocr_indexing_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/ocr_indexing_options/)) specify how the text of the document is extracted; they are necessary if the document text was not saved in the index.
*   [use_inline_styles](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/use_inline_styles/) is a flag to specify whether inline styles or CSS class are used to highlight occurrences. The default value is True.
*   [highlight_color](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/highlight_color/) is a color used to highlight occurrences. The default value is (255, 216, 0).
*   [term_highlight_start_tag](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/term_highlight_start_tag/) is the start tag of the highlighting of the found word. This tag is used only when highlighting in plain text.
*   [term_highlight_end_tag](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/term_highlight_end_tag/) is the end tag of the highlighting of the found word. This tag is used only when highlighting in plain text.

The other options are used for highlighting occurrences in text fragments.

To highlight search results in the text of the whole document, a highlighter of the [DocumentHighlighter](https://reference.groupdocs.com/search/python-net/groupdocs.search.highlighters/documenthighlighter/) class is used. To create a highlighter of this type, you must pass an object of a class derived from the abstract class [OutputAdapter](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/outputadapter/) to its constructor. Details on the output adapters are presented on the page [Output adapters]({{< ref "search/python-net/developer-guide/advanced-usage/searching/output-adapters.md" >}}).

If after generation the text of a document was saved to a file, this file can be opened by an Internet browser to navigate the occurrences of the words found. To navigate the occurrences, the following text is added to the URL in a browser:

#hitN

where N is a number starting from zero. The full document URL in a browser might look like this:

[file:///C:/Text.html#hit0](file:///C:/Text.html#hit0)

The following example demonstrates how to highlight search results in the text of an entire document.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, IndexSettings
from groupdocs.search.common import StructureOutputAdapter
from groupdocs.search.highlighters import DocumentHighlighter
from groupdocs.search.options import Compression, HighlightOptions, OutputFormat, TextStorageSettings

# Creating an index settings instance
settings = IndexSettings()
settings.text_storage_settings = TextStorageSettings(Compression.HIGH)  # Enabling storage of extracted text in the index

# Creating an index in the specified folder
index = Index("./index", settings)

# Indexing documents from the specified folder
index.add("./documents")

# Search for the word 'Universe'
result = index.search("Universe")

# Highlighting occurrences in the text
if result.document_count > 0:
    document = result.get_found_document(0)  # Getting the first found document
    output_adapter = StructureOutputAdapter(OutputFormat.PLAIN_TEXT)  # Creating the output adapter
    highlighter = DocumentHighlighter(output_adapter)  # Creating the highlighter instance
    options = HighlightOptions()  # Creating the highlight options
    options.term_highlight_start_tag = "<Term>"  # Setting the start tag for the found word
    options.term_highlight_end_tag = "</Term>"  # Setting the end tag for the found word
    index.highlight(document, highlighter, options)  # Generating plain text with highlighted occurrences

    fields = output_adapter.get_result()
    print(document.to_string())
    for field in fields:
        # Printing field names of the found document
        print("\t" + field.name)

    # Saving the plain text of the fields with highlighted occurrences to a file
    os.makedirs("./output", exist_ok=True)
    with open("./output/Highlighted.txt", "w", encoding="utf-8") as file:
        for field in fields:
            file.write(field.name + ": " + field.value + "\n")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/highlighting-search-results/documents/einstein-biography.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/highlighting-search-results/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Highlighted.txt" >}}  
```text
filename: documents\einstein-biography.txt
formatfamily: wordprocessing
creationdate: 10/29/2026 19:06:04
modificationdate: 10/29/2026 18:00:18
content: Albert Einstein was a theoretical physicist who developed the theory of relativity.
In 1905 Einstein published his first paper on relativity, and in 1915 he completed the general theory of relativity.
His work changed our picture of the <Term>Universe</Term>: space and time form one fabric that mass can bend.
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/highlighting-search-results/hit_highlighting_in_the_text_of_entire_document/Highlighted.txt)
{{< /tab >}}
{{< /tabs >}}

## Hit highlighting in text fragments

Occurrences can also be highlighted in separate HTML or plain text fragments. For this, the highlighter of the [FragmentHighlighter](https://reference.groupdocs.com/search/python-net/groupdocs.search.highlighters/fragmenthighlighter/) class is used. The following properties are presented in the [HighlightOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/) class to use with the highlighter of this type:

*   [terms_before](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/terms_before/) is used to specify the maximum number of words in a text fragment before highlighted word. The value must be in the range from 0 to 10000. The default value is 7.
*   [terms_after](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/terms_after/) is used to specify the maximum number of words in a text fragment after highlighted word. The value must be in the range from 0 to 10000. The default value is 7.
*   [terms_total](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/terms_total/) is used to specify the maximum number of words in a text fragment. The value must be in the range from 0 to 10000. The default value is 21.
*   [term_highlight_start_tag](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/term_highlight_start_tag/) is the start tag of the highlighting of the found word. This tag is used only when highlighting in plain text.
*   [term_highlight_end_tag](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/highlightoptions/term_highlight_end_tag/) is the end tag of the highlighting of the found word. This tag is used only when highlighting in plain text.

Generated text fragments with highlighted occurrences can be used, for example, to generate web pages containing search results on a site.

The example below demonstrates how to highlight search results in separate text fragments.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, IndexSettings
from groupdocs.search.highlighters import FragmentHighlighter
from groupdocs.search.options import Color, Compression, HighlightOptions, OutputFormat, TextStorageSettings

# Creating an index settings instance
settings = IndexSettings()
settings.text_storage_settings = TextStorageSettings(Compression.HIGH)  # Enabling storage of extracted text in the index

# Creating an index in the specified folder
index = Index("./index", settings)

# Indexing documents from the specified folder
index.add("./documents")

# Search for the word 'Einstein'
result = index.search("Einstein")

# Assigning highlight options
options = HighlightOptions()
options.terms_before = 5
options.terms_after = 5
options.terms_total = 15
options.highlight_color = Color(0, 0, 127)
options.use_inline_styles = True

if result.document_count > 0:
    # Highlighting found words in separate text fragments of a document
    document = result.get_found_document(0)  # Getting the first found document
    highlighter = FragmentHighlighter(OutputFormat.HTML)
    index.highlight(document, highlighter, options)

    # Getting the result
    os.makedirs("./output", exist_ok=True)
    with open("./output/Fragments.html", "w", encoding="utf-8") as file:
        fragment_containers = highlighter.get_result()
        for container in fragment_containers:
            fragments = container.get_fragments()
            if len(fragments) > 0:
                print(container.field_name)
                print()
                for fragment in fragments:
                    # Printing HTML markup to console and saving it to the file
                    print(fragment)
                    print()
                    file.write("<p>" + fragment + "</p>\n")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/highlighting-search-results/documents/einstein-biography.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/highlighting-search-results/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Fragments.html" >}}  
```text
<p>documents\<a title="einstein" name="hit0"><span style="background-color: rgba(0, 0, 127, 1);">einstein</span></a>-biography.txt</p>
<p>/2026 18:00:18Albert <a title="einstein" name="hit1"><span style="background-color: rgba(0, 0, 127, 1);">Einstein</span></a> was a theoretical physicist who</p>
<p> theory of relativity.<br>

In 1905 <a title="einstein" name="hit2"><span style="background-color: rgba(0, 0, 127, 1);">Einstein</span></a> published his first paper on</p>
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/highlighting-search-results/hit_highlighting_in_text_fragments/Fragments.html)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
