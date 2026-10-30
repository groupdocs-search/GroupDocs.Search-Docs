---
id: indexing-password-protected-documents
url: search/python-net/indexing-password-protected-documents
title: Indexing password protected documents
weight: 16
description: "This article explains how to index password protected documents using the password dictionary or the PasswordRequired event."
keywords: indexing of password protected documents, document passwords, password dictionary, password_required, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
structuredData:
  showOrganization: True
  application:
    name: Document Search
    description: Search documents natively with high performance using Python language and GroupDocs.Search for Python via .NET
    productCode: search
    productPlatform: python-net
  showVideo: True
  howTo:
    name: How to index password protected documents in Python
    description: Learn how to index password protected documents in Python step by step
    steps:
      - name: Create an index
        text: Create an instance of the Index class in the folder of the index.
      - name: Provide the passwords
        text: Add the password of each protected document to the password dictionary of the index, or set it in a handler of the password_required event.
      - name: Index the documents
        text: Call the add method of the index with the folder of the documents, and search in the index.
---
This page describes how to perform indexing of password protected documents.

## Indexing using the password dictionary

To perform indexing of password protected documents using a password dictionary, you must add passwords for all protected documents to the dictionary before indexing. To add a document password to the dictionary, you must specify the full path to the document (or the document key for a document added from a stream) as a key, for example, a path returned by `os.path.abspath`, and the actual password to the document. The password dictionary can be changed at any time. For more information about managing the password dictionary see the [Document passwords]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/document-passwords.md" >}}) page in the [Managing dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The password dictionary is stored on disk in encrypted form. However, very simple encryption is used, so if you want to protect the passwords themselves, it is better to use the [password_required](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/password_required/) event to set passwords.

The following example demonstrates how to perform indexing of password protected documents using a password dictionary.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index

# Creating an index
index = Index("./index")

# Adding document passwords to the dictionary
# (an index opened again keeps its dictionary, so the password may be there already)
key = os.path.abspath("./documents/ProtectedDocument.docx")
if not index.dictionaries.document_passwords.contains(key):
    index.dictionaries.document_passwords.add(key, "123456")
# ...

# Indexing documents from the specified folder
# Passwords will be automatically retrieved from the dictionary when necessary
index.add("./documents")

result = index.search("strikethrough")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-password-protected-documents/documents/Newton.txt), [documents/ProtectedDocument.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-password-protected-documents/documents/ProtectedDocument.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-using-the-password-dictionary.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-password-protected-documents/indexing_using_the_password_dictionary/indexing-using-the-password-dictionary.txt)
{{< /tab >}}
{{< /tabs >}}

## Indexing using the password_required event

The password for the protected document can also be provided when the **[password_required](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/password_required/)** event occurs. This method is more secure; however, it requires matching the indexed documents and their passwords in the event handler.

Both of the described methods for providing document passwords can be used together. The index first checks for a document password in the password dictionary, and then, if the password is absent, raises the **[password_required](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/password_required/)** event. If the password is not provided or an incorrect password is provided, the index will raise the **[error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/)** event with the corresponding error message and the document will not be indexed. You can learn more about index events on the page [Search index events]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}).

The handler of the event receives the sender and the event arguments, and sets the [password](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/passwordrequiredeventargs/password/) property of the arguments. The following example shows how to provide password for a document using the **[password_required](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/password_required/)** event.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index

# Creating an index
index = Index("./index")


# Subscribing to the event
def on_password_required(sender, args):
    if args.document_full_path.lower().endswith("protecteddocument.docx"):
        args.password = "123456"  # Providing password for the file 'ProtectedDocument.docx'


index.events.password_required += on_password_required

# Indexing documents from the specified folder
index.add("./documents")

result = index.search("strikethrough")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-password-protected-documents/documents/Newton.txt), [documents/ProtectedDocument.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-password-protected-documents/documents/ProtectedDocument.docx)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-using-the-password-required-event.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-password-protected-documents/indexing_using_the_password_required_event/indexing-using-the-password-required-event.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
