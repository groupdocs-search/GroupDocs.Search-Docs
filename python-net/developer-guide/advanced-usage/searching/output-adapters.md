---
id: output-adapters
url: search/python-net/output-adapters
title: Output adapters
weight: 13
description: "This article describes output adapters, which are used to output generated HTML or plain text to various output objects."
keywords: Output adapters
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Output adapters are used to output generated HTML or plain text to various output objects. The output format (HTML or plain text) is specified by the [OutputFormat](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/outputformat/) value passed to the constructor of an adapter. The following output adapters are currently supported:

*   [FileOutputAdapter](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/fileoutputadapter/) is an output adapter that is used to output text to a file. The path to a file is specified in the constructor of the adapter.
*   [StreamOutputAdapter](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/streamoutputadapter/) is an output adapter that is used to output text to a stream. The stream is specified in the constructor of the adapter; in Python, pass a binary file object opened for writing, for example, the one returned by `open(path, "wb")`.
*   [StringOutputAdapter](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/stringoutputadapter/) is an output adapter that is used to output text to a string. The resulting string can be accessed through the [get_result](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/stringoutputadapter/get_result/) method of the adapter class.
*   [StructureOutputAdapter](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/structureoutputadapter/) is an output adapter that is used to output the text of each field of the document separately. The resulting list of fields can be accessed through the [get_result](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/structureoutputadapter/get_result/) method of the adapter class.

The example below demonstrates how to use adapters of different types.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, IndexSettings
from groupdocs.search.common import FileOutputAdapter, StreamOutputAdapter, StringOutputAdapter, StructureOutputAdapter
from groupdocs.search.options import Compression, OutputFormat, TextStorageSettings

# Creating an index settings instance
settings = IndexSettings()
settings.text_storage_settings = TextStorageSettings(Compression.HIGH)  # Enabling storage of extracted text in the index

# Creating an index in the specified folder
index = Index("./index", settings)

# Indexing documents from the specified folder
index.add("./documents")

# Getting list of indexed documents
documents = index.get_indexed_documents()

# Getting a document text
if len(documents) > 0:
    document = documents[0]
    os.makedirs("./output", exist_ok=True)

    # Output to a file
    file_output_adapter = FileOutputAdapter(OutputFormat.HTML, "./output/Text.html")
    index.get_document_text(document, file_output_adapter)

    # Output to a stream
    with open("./output/Stream.html", "wb") as stream:
        stream_output_adapter = StreamOutputAdapter(OutputFormat.HTML, stream)
        index.get_document_text(document, stream_output_adapter)

    # Output to a string
    string_output_adapter = StringOutputAdapter(OutputFormat.HTML)
    index.get_document_text(document, string_output_adapter)
    html_text = string_output_adapter.get_result()
    print(html_text)

    # Output to a structure
    structure_output_adapter = StructureOutputAdapter(OutputFormat.PLAIN_TEXT)
    index.get_document_text(document, structure_output_adapter)
    fields = structure_output_adapter.get_result()
    print(document.to_string())
    for field in fields:
        print("\t" + field.name)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample file is used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/output-adapters/documents/einstein-biography.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Stream.html" >}}  
```text
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>documents\einstein-biography.txt</title>
</head>
<body>
<p>documents\einstein-biography.txt</p>
<p></p>
<h3>filename</h3>
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/output-adapters/output_adapters/Stream.html)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
