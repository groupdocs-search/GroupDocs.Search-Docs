---
id: indexing-additional-fields
url: search/python-net/indexing-additional-fields
title: Indexing additional fields
weight: 12
description: "This article explains how to associate indexed documents with additional fields, such as tags or subjects."
keywords: additional fields, additional metadata, file_indexing, DocumentField, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Sometimes when indexing, it is necessary to associate each document with certain additional metadata, for example, a set of tags, a number in the library catalog, the subject of a document, etc. To accomplish this task, additional fields can be added to each indexed document in addition to those already in the document itself.

Additional fields are associated with the document through the arguments of the [file_indexing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/file_indexing/) event that occurs before indexing each added document. When adding each field, its name and contents are specified. The name of the added field can match the names of other added fields or fields of the document itself. When searching in a field, matches in all fields with the specified name are taken into account. For documents added with the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/add/) method from a list of [Document](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/document/) objects, additional fields can also be set through the [additional_fields](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/document/additional_fields/) property of a document before indexing.

The example below demonstrates the associating of additional fields with documents during indexing. The handler of the event sets the [additional_fields](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/fileindexingeventargs/additional_fields/) property of the event arguments to a list of [DocumentField](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/documentfield/) objects, and the example then searches in the added field.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import DocumentField

# Defining somewhere a dictionary containing subjects of documents
subjects = {}
subjects[os.path.abspath("./documents/Bhagavad-Gita-lecture.pdf").lower()] = "Spiritual"
# ...

# Creating an index
index = Index("./index")


# Subscribing to the event
def on_file_indexing(sender, args):
    subject = subjects.get(args.document_full_path.lower())  # Getting a subject for the current document
    if subject is not None:
        args.additional_fields = [  # Setting additional fields for the current document
            DocumentField("Subject", subject)
        ]


index.events.file_indexing += on_file_indexing

# Indexing documents from the specified folder
index.add("./documents")

# Searching in the added field
result = index.search("Subject:Spiritual")
for found in result:
    print("Found:", os.path.basename(found.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-additional-fields/documents/Einstein.txt), [documents/Bhagavad-Gita-lecture.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-additional-fields/documents/Bhagavad-Gita-lecture.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-additional-fields.txt" >}}  
```text
Found: Bhagavad-Gita-lecture.pdf
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-additional-fields/indexing_additional_fields/indexing-additional-fields.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
