---
id: text-file-encoding-detection
url: search/python-net/text-file-encoding-detection
title: Text file encoding detection
weight: 25
description: "This article explains how to detect or specify the encoding of text files during indexing."
keywords: detect encoding of a text file, text file encoding, auto_detect_encoding, Encodings, chardet, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
To automatically detect the encoding of a text file, use the [auto_detect_encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/auto_detect_encoding/) property of the [IndexingOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/) class, which is inherited from the [BaseOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/) class and is also available in the [UpdateOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/updateoptions/) class. Setting this property to `True` allows you to detect the following encodings:

*   UTF-32 LE;
*   UTF-32 BE;
*   UTF-16 LE;
*   UTF-16 BE;
*   UTF-8;
*   UTF-7.

By default, the encoding auto detection of text files is disabled. But in any case, the encoding of a text file can be set during indexing when the [file_indexing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/file_indexing/) event is raised. If the encoding of a text file has not been detected or specified in the event arguments, then the default encoding is used: the value of the [encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/baseoptions/encoding/) property of the indexing options or, if it is `None`, UTF-8. Available encodings are presented in the [Encodings](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/encodings/) class, whose constants are the names of the encodings, for example `Encodings.WINDOWS_1253` is `"Windows-1253"`. The handler of the event sets the [encoding](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/fileindexingeventargs/encoding/) property of the event arguments to such a name. When the encoding of a text file is detected and used for indexing, it is saved in the index to use in such methods of [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class like [highlight](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/highlight/) and [get_document_text](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_document_text/).

The example below shows how to set the encoding of text files during indexing; the documents are Greek texts in the Windows-1253 encoding.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.common import Encodings

# Creating an index
index = Index("./index")


# Subscribing to the event
def on_file_indexing(sender, args):
    if args.document_full_path.lower().endswith(".txt"):
        args.encoding = Encodings.WINDOWS_1253  # Setting encoding for each text file


index.events.file_indexing += on_file_indexing

# Indexing documents from the specified folder
index.add("./documents")

# Searching for a Greek word ("relativity")
result = index.search("σχετικότητας")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/text-file-encoding-detection/documents/Newton.txt), [documents/Greek.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/text-file-encoding-detection/documents/Greek.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "text-file-encoding-detection.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/text-file-encoding-detection/text_file_encoding_detection/text-file-encoding-detection.txt)
{{< /tab >}}
{{< /tabs >}}

External tools, such as the [chardet](https://pypi.org/project/chardet/) package, can be used to determine the encoding of a text file during indexing.
Any encoding can be detected, including ANSI.

```bash
pip install chardet
```

Below is an example of using the external library to determine the encoding of a text file. The names that chardet returns, such as `Windows-1253`, are names that the index accepts. Without chardet installed, the example indexes the files with the default encoding.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

try:
    import chardet  # pip install chardet
except ImportError:
    chardet = None

# Creating an index
index = Index("./index")


# Subscribing to the event
def on_file_indexing(sender, args):
    if args.document_full_path is not None and args.document_full_path.lower().endswith(".txt"):
        if chardet is None:
            print("chardet is not installed: the default encoding is used")
            return
        with open(args.document_full_path, "rb") as f:
            data = f.read()
        result = chardet.detect(data)
        if result["encoding"] is not None:
            print("Encoding detected: " + result["encoding"])
            args.encoding = result["encoding"]


index.events.file_indexing += on_file_indexing

# Indexing documents from the specified folder
index.add("./documents")
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/text-file-encoding-detection/documents/Newton.txt), [documents/Greek.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/text-file-encoding-detection/documents/Greek.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "text-file-encoding-detection-2.txt" >}}  
```text
chardet is not installed: the default encoding is used
chardet is not installed: the default encoding is used
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/text-file-encoding-detection/text_file_encoding_detection_2/text-file-encoding-detection-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
