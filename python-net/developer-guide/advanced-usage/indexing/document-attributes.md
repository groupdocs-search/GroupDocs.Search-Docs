---
id: document-attributes
url: search/python-net/document-attributes
title: Document attributes
weight: 8
description: "Document attributes is a special feature designed for marking indexed documents with text labels without the need for re-indexing."
keywords: document attributes, marking indexed documents, AttributeChangeBatch, change_attributes, filter by attribute, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Document attributes is a special feature designed for marking indexed documents with text labels without the need for re-indexing. Added attributes can be further used to filter documents during the search.

To add and delete attributes of indexed documents, use the [change_attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/change_attributes/) method of the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) class. This method accepts an [AttributeChangeBatch](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/attributechangebatch/) object containing the required attribute changes as a parameter. The [AttributeChangeBatch](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/attributechangebatch/) class provides the following methods:

*   [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/attributechangebatch/add/) adds the specified attributes to one or several indexed documents;
*   [add_to_all](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/attributechangebatch/add_to_all/) adds the specified attributes to all documents in the index;
*   [remove](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/attributechangebatch/remove/) removes the specified attributes from one or several indexed documents;
*   [remove_all](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/attributechangebatch/remove_all/) removes all attributes from one or several indexed documents;
*   [remove_from_all](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/attributechangebatch/remove_from_all/) removes the specified attributes from all documents in the index;
*   [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/attributechangebatch/clear/) removes all attributes from all documents in the index. Note that this method does not clear the batch, but adds a change to it.

In Python, the attributes are passed to these methods as a list, for example `batch.add_to_all(["public"])`, and a document is identified by its full path, for example, a path returned by `os.path.abspath`. The changes are applied in the order in which they are added to the batch. The attributes of an indexed document can be obtained with the [get_attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/get_attributes/) method.

The following example demonstrates how to add and remove attributes from indexed documents.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import AttributeChangeBatch
from groupdocs.search.options import SearchDocumentFilter, SearchOptions

# Creating an index
index = Index("./index")

# Indexing documents in a document folder
index.add("./documents")

# The full path of a document identifies it in the index
key_points = os.path.abspath("./documents/KeyPoints.docx")

# Creating an attribute change container object
batch = AttributeChangeBatch()
# Adding one attribute to all indexed documents
batch.add_to_all(["public"])
# Removing one attribute from one indexed document
batch.remove(key_points, ["public"])
# Adding two attributes to one indexed document
batch.add(key_points, ["main", "key"])

# Applying attribute changes in the index
index.change_attributes(batch)

# Searching in the index
options = SearchOptions()
# Creating a document filter by attribute
options.search_document_filter = SearchDocumentFilter.create_attribute("main")
result = index.search("Einstein", options)
for found in result:
    print("Found:", os.path.basename(found.document_info.file_path))
print("Attributes of KeyPoints.docx:", ", ".join(index.get_attributes(key_points)))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-attributes/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-attributes/documents/Newton.txt), [documents/KeyPoints.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-attributes/documents/KeyPoints.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "document-attributes.txt" >}}  
```text
Found: KeyPoints.docx
Attributes of KeyPoints.docx: main, key
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-attributes/document_attributes/document-attributes.txt)
{{< /tab >}}
{{< /tabs >}}

Attributes can be associated with documents during indexing using the [file_indexing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/file_indexing/) event: the handler sets the [attributes](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/fileindexingeventargs/attributes/) property of the event arguments to a list of strings. The following example demonstrates this.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchDocumentFilter, SearchOptions

# Creating an index
index = Index("./index")


# Subscribing to the file_indexing event for adding attributes
def on_file_indexing(sender, args):
    if args.document_full_path.endswith("KeyPoints.docx"):
        # Adding two attributes
        args.attributes = ["main", "key"]


index.events.file_indexing += on_file_indexing

# Indexing documents in a document folder
index.add("./documents")

# Searching in the index
options = SearchOptions()
# Creating a document filter by attribute
options.search_document_filter = SearchDocumentFilter.create_attribute("main")
result = index.search("Einstein", options)
for found in result:
    print("Found:", os.path.basename(found.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-attributes/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-attributes/documents/Newton.txt), [documents/KeyPoints.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-attributes/documents/KeyPoints.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "document-attributes-2.txt" >}}  
```text
Found: KeyPoints.docx
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-attributes/document_attributes_2/document-attributes-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
