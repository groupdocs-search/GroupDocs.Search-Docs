---
id: custom-text-extractors
url: search/python-net/custom-text-extractors
title: Custom text extractors
weight: 4
description: "This article explains how to implement a custom text extractor to index documents of any format, including container documents."
keywords: custom text extractor, IFieldExtractor, IContainerItemExtractor, unsupported formats, custom_extractors, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
GroupDocs.Search supports indexing of many document formats. In addition, you can implement support for any other format, or replace the built-in extraction for a supported format, with a custom text extractor.

A custom text extractor is a class implementing the [IFieldExtractor](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/ifieldextractor/) interface. In Python, it is a plain class with the members of the interface; do not derive it from the `IFieldExtractor` class. The [extensions](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/ifieldextractor/extensions/) attribute lists the file extensions the extractor is used for, and the [get_fields](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/ifieldextractor/get_fields/) method extracts the fields of a document from a file or from a stream. For a file, the method receives the path to the file as a `str`; for a document added from a stream, it receives the stream, whose `read()` method returns the bytes of the document. The method returns a list of [DocumentField](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/documentfield/) objects or `(name, value)` tuples.

The following example demonstrates how to implement a custom text extractor.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import datetime
import os

from groupdocs.search.common import DocumentField


class LogExtractor(object):
    extensions = [".log"]

    def get_fields(self, source):
        if isinstance(source, str):
            # The document is a file: source is the path to the file
            creation_time = datetime.datetime.fromtimestamp(os.path.getctime(source))
            return [
                DocumentField("FileName", os.path.abspath(source)),
                DocumentField("CreationDate", creation_time.strftime("%m/%d/%Y %H:%M:%S")),
                DocumentField("Content", self.extract_content(source)),
            ]
        # The document is a stream: source.read() returns its bytes
        # The stream belongs to the caller, so it is not closed here
        return [DocumentField("Content", source.read().decode("utf-8-sig"))]

    def extract_content(self, file_path):
        with open(file_path, encoding="utf-8-sig") as f:
            return f.read()
```
{{< /tab >}}
{{< /tabs >}}

To use a custom extractor, add it to the [custom_extractors](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/custom_extractors/) collection of the index settings. A custom extractor takes precedence over the built-in extractor for the same extension. Custom extractors are not saved in an index, so they must be added each time the index is created or opened. The next example shows how to use the custom extractor for indexing.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import datetime
import os

from groupdocs.search import Index, IndexSettings
from groupdocs.search.common import DocumentField


class LogExtractor(object):
    extensions = [".log"]

    def get_fields(self, source):
        if isinstance(source, str):
            creation_time = datetime.datetime.fromtimestamp(os.path.getctime(source))
            return [
                DocumentField("FileName", os.path.abspath(source)),
                DocumentField("CreationDate", creation_time.strftime("%m/%d/%Y %H:%M:%S")),
                DocumentField("Content", self.extract_content(source)),
            ]
        return [DocumentField("Content", source.read().decode("utf-8-sig"))]

    def extract_content(self, file_path):
        with open(file_path, encoding="utf-8-sig") as f:
            return f.read()


settings = IndexSettings()
settings.custom_extractors.add(LogExtractor())  # Adding custom text extractor to the index settings

index = Index("./index", settings)  # Creating or loading an index

index.add("./documents")  # Indexing documents from the specified folder

result = index.search("quota")  # Searching for a word from the log file
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/custom-text-extractors/documents/Einstein.txt), [documents/server.log](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/custom-text-extractors/documents/server.log), [documents/planets.lines](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/custom-text-extractors/documents/planets.lines)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "custom-text-extractors-2.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/custom-text-extractors/custom_text_extractors_2/custom-text-extractors-2.txt)
{{< /tab >}}
{{< /tabs >}}

A custom extractor can also be specified for an individual document in the [file_indexing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/file_indexing/) event handler through the [custom_extractor](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/fileindexingeventargs/custom_extractor/) property of the event arguments (see [Search index events]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}})), and for [separate data extraction]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/separate-data-extraction.md" >}}) through the [ExtractionOptions.custom_extractor](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/extractionoptions/custom_extractor/) property.

## Custom extractors for container documents

To index a container document (for example, an archive) whose items are indexed as separate items of the document, implement the [IContainerItemExtractor](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/icontaineritemextractor/) interface. In addition to the members of [IFieldExtractor](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/ifieldextractor/), this interface contains:

*   The [format_family](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/icontaineritemextractor/format_family/) attribute, the format family of the container document: a constant of the [FormatFamily](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/formatfamily/) class, for example `FormatFamily.ARCHIVE`.
*   The [get_container_items](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/icontaineritemextractor/get_container_items/) method, which extracts the items of a container document from a file or from a stream (it receives a path or a stream, as `get_fields` does) and returns them as a list of [ExtractedItemInfo](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractediteminfo/) objects or `(format_family, fields, item_info, inner_path)` tuples.

Each [ExtractedItemInfo](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractediteminfo/) object describes one item: its [format_family](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractediteminfo/format_family/), the extracted [fields](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractediteminfo/fields/), the [item_info](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractediteminfo/item_info/) string that identifies the item in the container, and the [inner_path](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractediteminfo/inner_path/) of the item. Items with an empty [item_info](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/extractediteminfo/item_info/) value are not indexed. The fields returned by [get_fields](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/ifieldextractor/get_fields/) are indexed as the fields of the container document itself. The indexed items of a container document can be obtained with the [get_indexed_document_items](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_indexed_document_items/) method.

The following example demonstrates a custom extractor that indexes each line of a text file as a separate item. The example then indexes a folder with such files and prints the indexed items; the `item_info` string of an item is the `inner_path` of its [DocumentInfo](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/).

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, IndexSettings
from groupdocs.search.common import DocumentField, ExtractedItemInfo
from groupdocs.search.results import FormatFamily


class LineContainerExtractor(object):
    extensions = [".lines"]
    format_family = FormatFamily.ARCHIVE

    def get_fields(self, source):
        if isinstance(source, str):
            # The container document itself has only the file name field
            return [DocumentField("FileName", os.path.basename(source))]
        return []

    def get_container_items(self, source):
        if isinstance(source, str):
            with open(source, encoding="utf-8-sig") as f:
                lines = f.read().splitlines()
        else:
            lines = source.read().decode("utf-8-sig").splitlines()
        return self.create_items(lines)

    @staticmethod
    def create_items(lines):
        items = []
        for i, line in enumerate(lines):
            fields = [DocumentField("Content", line)]
            item_info = "Line " + str(i + 1)
            items.append(ExtractedItemInfo(FormatFamily.WORD_PROCESSING, fields, item_info, ""))
        return items


# Indexing the lines of the .lines files as separate items
settings = IndexSettings()
settings.custom_extractors.add(LineContainerExtractor())
index = Index("./index", settings)
index.add("./documents")

# Getting the indexed items of each container document
for document in index.get_indexed_documents():
    print(os.path.basename(document.file_path))
    for item in index.get_indexed_document_items(document):
        print("\t" + item.inner_path)

# Searching in the items
result = index.search("Venus")
for found in result:
    print("Found in:", found.document_info.inner_path)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/custom-text-extractors/documents/Einstein.txt), [documents/server.log](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/custom-text-extractors/documents/server.log), [documents/planets.lines](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/custom-text-extractors/documents/planets.lines)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "custom-extractors-container-documents.txt" >}}  
```text
Einstein.txt
planets.lines
	Line 1
	Line 2
	Line 3
server.log
Found in: Line 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/custom-text-extractors/custom_extractors_for_container_documents/custom-extractors-container-documents.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
