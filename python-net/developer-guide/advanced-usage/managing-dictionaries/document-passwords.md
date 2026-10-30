---
id: document-passwords
url: search/python-net/document-passwords
title: Document passwords
weight: 4
description: "This article describes the Python API for managing the password dictionary that stores passwords of documents to be indexed."
keywords: document passwords, password dictionary
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The [PasswordDictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/passworddictionary/) class is designed to store passwords for documents to be indexed. The password dictionary of an index is available through the [document_passwords](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionaryrepository/document_passwords/) property of the [Index.dictionaries](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/dictionaries/) object. Information on indexing password protected documents is presented on the [Indexing password protected documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-password-protected-documents.md" >}}) page.

The password dictionary can be changed at any time. A password is stored with a key, which is the full path of a document file or, for a document added from a stream or a structure, the document key.

To get the number of passwords in the dictionary, use the [count](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/passworddictionary/count/) property.

To get the password for a document from the dictionary, the [get_password](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/passworddictionary/get_password/) method is used, passing the full path or the key of the document as an argument.

The [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/passworddictionary/clear/) method is used to remove all passwords from the dictionary.

To check for the presence of a password in the dictionary for the specified document, the [contains](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/passworddictionary/contains/) method is used.

To add a password to the dictionary, use the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/passworddictionary/add/) method. The first parameter is the full path or the key of the document. If the dictionary already contains a password for the document, the method raises `ValueError`; remove the existing password first.

To remove a password from the dictionary, use the [remove](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/passworddictionary/remove/) method.

To export passwords to a file, use the [export_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/export_dictionary/) method.

To import passwords from a file, use the [import_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionarybase/import_dictionary/) method. The imported passwords are added to the dictionary, so the dictionary must not already contain passwords for the same documents; otherwise, the import fails. Errors of exporting and importing are reported by the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the password dictionary.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index

# Creating or opening an index in the specified folder
index = Index("./index")

passwords = index.dictionaries.document_passwords

path = os.path.abspath("./documents/protected.pdf")
if passwords.contains(path):
    # Getting a password for a document
    password = passwords.get_password(path)
    print(path)
    print("\tPassword: " + password)

    # Deleting the password from the dictionary
    passwords.remove(path)

if passwords.count > 0:
    # Removing all passwords from the dictionary
    passwords.clear()

# Adding a password for a document
passwords.add(path, "123456")

# Exporting passwords to a file
passwords.export_dictionary("./Passwords.dat")

# Removing all passwords and importing them from the file
passwords.clear()
passwords.import_dictionary("./Passwords.dat")

# Indexing documents; the passwords from the dictionary are used to open protected documents
index.add("./documents")

# Searching for a word of the protected document
result = index.search("confidential")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/document-passwords/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/document-passwords/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/document-passwords/documents/curie.txt), [documents/protected.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/managing-dictionaries/document-passwords/documents/protected.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "document-passwords.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/managing-dictionaries/document-passwords/document_passwords/document-passwords.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
