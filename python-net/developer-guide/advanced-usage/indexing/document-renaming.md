---
id: document-renaming
url: search/python-net/document-renaming
title: Document renaming
weight: 10
description: "This article explains how to notify an index about a renamed document so that the document is not reindexed during the update operation."
keywords: document renaming, rename notification, notify, update index, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Sometimes a situation arises when an indexed document is renamed, but its contents have not changed. In this case, to save computing resources, you can notify the index about the renaming of the document, and then the document will not be reindexed during the update operation.

To notify an index about renaming a document, the [notify](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/notify/) method is used with a notification object created by the [Notification.create_rename_notification](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/notification/create_rename_notification/) method as a parameter. The [notify](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/notify/) method returns `True` if the notification was successfully performed.

You should keep in mind that if an index is notified of the renaming of a document, it will not be reindexed the next time you call the [update](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/update/) method, even if its contents have changed. The following example demonstrates how to notify an index of a renamed document.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import Notification

# Creating an index
index = Index("./index")

# Indexing documents in a document folder
index.add("./documents")

# Renaming a document
old_document_path = "./documents/OldDocumentName.txt"
new_document_path = "./documents/NewDocumentName.txt"
os.rename(old_document_path, new_document_path)

# Notifying the index about renaming
notification = Notification.create_rename_notification(old_document_path, new_document_path)
result = index.notify(notification)
print("Notification applied:", result)

# Updating the index
# The renamed document will not be reindexed
index.update()

for document in index.get_indexed_documents():
    print("Indexed:", os.path.basename(document.file_path))

# Restoring the original file name, so that the example can run again
os.rename(new_document_path, old_document_path)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/OldDocumentName.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-renaming/documents/OldDocumentName.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/document-renaming/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "document-renaming.txt" >}}  
```text
Notification applied: True
Indexed: Newton.txt
Indexed: NewDocumentName.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/document-renaming/document_renaming/document-renaming.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
