---
id: get-supported-file-formats
url: search/python-net/get-supported-file-formats
title: Get supported file formats
weight: 5
description: "This article describes how to obtain the list of file types supported by GroupDocs.Search for Python via .NET."
keywords: supported file types, supported formats, FileType, get_supported_file_types, Python
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
    name: Get file formats supported by Search in Python
    description: Get file formats supported by Search in Python step by step
    steps:
      - name: Get the list of supported file types
        text: Call the get_supported_file_types method of the FileType class. The result is a list of FileType objects.
      - name: Output the supported file types
        text: Skip FileType.UNKNOWN, sort the list by extension and output the extension and the description of each file type, for example, to the console.
---
The [get_supported_file_types](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/filetype/get_supported_file_types/) method of the [FileType](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/filetype/) class is used to obtain a list of supported file types.

The returned list also contains the [FileType.UNKNOWN](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/filetype/unknown/) value, which has an empty extension. An example of obtaining a list of supported file types is presented below. The example filters and sorts the list with a generator expression and the built-in *sorted* function.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search.results import FileType

supported_file_types = sorted(
    (ft for ft in FileType.get_supported_file_types() if ft != FileType.UNKNOWN),  # Skipping the unknown file type
    key=lambda ft: ft.extension)

for file_type in supported_file_types:
    print(file_type.extension.ljust(8) + " - " + file_type.description)
```
{{< /tab >}}
{{< tab "get-supported-file-formats.txt" >}}  
```text
.asf     - Advanced Systems Format File
.avi     - Audio Video Interleave File
.bmp     - Bitmap Picture
.chm     - Microsoft Compiled HTML Help
.csv     - Comma-Separated Values
.dc3     - Medicine Image File Format
.dcm     - DICOM Image
.dic     - DICOM Image
.dicom   - DICOM Image
.djv     - DjVu Image
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/basic-usage/get-supported-file-formats/get_supported_file_formats/get-supported-file-formats.txt)
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
