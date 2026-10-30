---
id: separate-data-extraction
url: search/python-net/separate-data-extraction
title: Separate data extraction
weight: 23
description: "This article explains how to separately extract data from documents and add the extracted data to the index."
keywords: separate data extraction, Extractor, ExtractedData, ExtractionOptions, serialize extracted data, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The feature described in this article makes it possible to separate the operations of extracting data from a document and adding the extracted data to the index. The extracted data can be easily serialized and deserialized as needed: the `serialize` method returns `bytes`, which can be stored or sent to another process or server, and `ExtractedData.deserialize` restores the data from them.

This feature can be useful because the operation of extracting text and other data from documents can be very long for some formats. At the same time, adding data to the index is a rather lightweight operation. Therefore, separating these operations significantly reduces the time during which the index is busy performing operations, and adding more data extractors increases the overall indexing performance. Data extractors can even work on separate servers, completely freeing the indexing server from extracting operations.

The [Extractor](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractor/) class is used to extract data from documents. Upon completion of the operation, the [extract](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractor/extract/) method returns an instance of the [ExtractedData](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extracteddata/) class, which is used directly to add to the index.

The [Extractor](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractor/) class has the following members in addition to the [extract](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractor/extract/) method:

*   The [settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractor/settings/) property returns an [ExtractorSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractorsettings/) object. Its [index_type](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractorsettings/index_type/) property specifies the type of the index for which data are extracted (the default value is `IndexType.NORMAL_INDEX`); set it to the type of the index to which the data will be added. Its [temp_folder](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractorsettings/temp_folder/) property specifies the folder for temporary files.
*   The [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractor/error_occurred/), [password_required](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractor/password_required/) and [image_preparing](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractor/image_preparing/) events, which work like the [index events]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}) with the same names.

The [ExtractionOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/extractionoptions/) class contains the same extraction options as the [IndexingOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/) class inherited from [BaseOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/) (see [Indexing options]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-options.md" >}})), as well as the [custom_extractor](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/extractionoptions/custom_extractor/) property, which specifies a [custom text extractor]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}) for the document.

The following example demonstrates how to perform separate data extraction and indexing.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Extractor, Index
from groupdocs.search.common import Document, ExtractedData
from groupdocs.search.options import ExtractionOptions, IndexingOptions

document_path = "./sample.pdf"

# Extracting data from a document
extractor = Extractor()
extractor.error_occurred += lambda sender, args: print(args.message)
document = Document.create_from_file(document_path)
extraction_options = ExtractionOptions()
extraction_options.use_raw_text_extraction = False
extracted_data = extractor.extract(document, extraction_options)

# Serializing the data
array = extracted_data.serialize()

# Deserializing the data
deserialized_data = ExtractedData.deserialize(array)

# Creating an index
index = Index("./index")

if deserialized_data.is_extraction_successful:
    # Indexing the data
    data = [
        deserialized_data
    ]
    index.add(data, IndexingOptions())

    # Searching in the index
    result = index.search("Einstein")
    print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/separate-data-extraction/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "separate-data-extraction.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/separate-data-extraction/separate_data_extraction/separate-data-extraction.txt)
{{< /tab >}}
{{< /tabs >}}

Note that when indexed documents change and need to be updated in the index, the same code must be executed. That is, data must be extracted separately from the updated documents and then the extracted data must be added to the index. The [update](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/update/) method does not update documents added from extracted data.

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
