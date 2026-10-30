---
id: indexing-from-different-sources
url: search/python-net/indexing-from-different-sources
title: Indexing from different sources
weight: 13
description: "This article explains how to index documents from files, streams and structures, as well as lazy-loaded documents from URL, FTP, Amazon S3 and Azure Blob Storage."
keywords: indexing documents from various sources, index from stream, index from structure, lazy loading, IDocumentLoader, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---

GroupDocs.Search allows indexing documents from various sources:

- From files in the file system.
- From a stream.
- From a data structure as a list of fields.

The library also allows indexing from all presented sources with lazy initialization: a document created with the [Document.create_lazy](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/document/create_lazy/) method is loaded by an [IDocumentLoader](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/idocumentloader/) implementation only when the index processes it. In Python, the loader is a plain class with the two methods of the interface; do not derive it from the `IDocumentLoader` class. The [load_document](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/idocumentloader/load_document/) method of the loader returns the loaded document, and the [close_document](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/idocumentloader/close_document/) method is called to release its resources. The loader is called for lazy documents of the `DocumentSourceKind.STREAM` and `DocumentSourceKind.STRUCTURE` kinds; a lazy document of the `DocumentSourceKind.FILE` kind is read from the file whose path is its key. If the loader raises an exception, the document is not indexed, and the error is reported through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event.

Please note that the update operation automatically generates a list of changed files only when indexing from the local file system. When indexing from streams or structures, documents cannot be updated with the update operation. To update documents from these sources, you must re-index the modified documents by passing their keys and updated data to the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/add/) method.

## Indexing from a file

It should be borne in mind that the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/add/) method with a list of [Document](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/document/) objects allows indexing only documents individually, and not entire folders. The advantage of indexing a list of documents is that you can add attributes and additional fields to the indexed document before calling the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/add/) method. The following example demonstrates how to index a document from a file.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.common import Document
from groupdocs.search.options import IndexingOptions

document_file_path = "./sample.pdf"

# Creating an index
index = Index("./index")

# Creating a document object
document = Document.create_from_file(document_file_path)
documents = [
    document,
]

# Indexing document from the file
options = IndexingOptions()
index.add(documents, options)

print("Documents found:", index.search("Einstein").document_count)
```
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-from-different-sources/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-file.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-from-different-sources/indexing_from_a_file/indexing-file.txt)
{{< /tab >}}
{{< /tabs >}}

## Indexing from a stream

The following example demonstrates how to index a document from a stream. A Python file object opened in binary mode can be passed as the stream; `io.BytesIO` works too.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import datetime

from groupdocs.search import Index
from groupdocs.search.common import Document
from groupdocs.search.options import IndexingOptions

document_file_path = "./sample.pdf"

# Creating an index
index = Index("./index")

# Creating a document object
stream = open(document_file_path, "rb")  # Opening a stream
document = Document.create_from_stream(document_file_path, datetime.datetime.now(), ".pdf", stream)
documents = [
    document,
]

# Indexing document from the stream
options = IndexingOptions()
index.add(documents, options)

# Closing the document stream after indexing is complete
stream.close()

print("Documents found:", index.search("Einstein").document_count)
```
{{< /tab >}}
{{< tab "sample.pdf" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.pdf](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-from-different-sources/sample.pdf)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-stream.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-from-different-sources/indexing_from_a_stream/indexing-stream.txt)
{{< /tab >}}
{{< /tabs >}}

## Indexing from a structure

The following example demonstrates how to index a document from a structure.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import datetime

from groupdocs.search import Index
from groupdocs.search.common import Document, DocumentField
from groupdocs.search.options import CommonFieldNames, IndexingOptions

document_file_path = "./sample.txt"

# Creating an index
index = Index("./index")

# Creating a document object
with open(document_file_path, encoding="utf-8") as f:
    text = f.read()
fields = [
    DocumentField(CommonFieldNames.CONTENT, text),
]
document = Document.create_from_structure("ExampleDocument", datetime.datetime.now(), fields)
documents = [
    document,
]

# Indexing document from the structure
options = IndexingOptions()
index.add(documents, options)

print("Documents found:", index.search("Einstein").document_count)
```
{{< /tab >}}
{{< tab "sample.txt" >}}
{{< tab-text >}}
The following sample file is used in this example: [sample.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/indexing-from-different-sources/sample.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "indexing-structure.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/indexing-from-different-sources/indexing_from_a_structure/indexing-structure.txt)
{{< /tab >}}
{{< /tabs >}}

## Indexing from URL

The following example demonstrates how to index a document by URL when lazy initialized. The document is downloaded with `urllib.request` from the Python standard library.

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
import datetime
import io
import urllib.request

from groupdocs.search import Index
from groupdocs.search.common import Document, DocumentSourceKind
from groupdocs.search.options import IndexingOptions


class DocumentLoaderFromUrl(object):
    def __init__(self, document_key, url, extension):
        self.document_key = document_key
        self.url = url
        self.extension = extension

    def load_document(self):
        with urllib.request.urlopen(self.url) as response:
            data = response.read()
        memory_stream = io.BytesIO(data)
        document = Document.create_from_stream(self.document_key, datetime.datetime.now(), self.extension, memory_stream)
        return document

    def close_document(self):
        pass


url = "https://example.com/ExampleDocument.pdf"

# Creating an index
index = Index("./index")
# Reporting errors, for example, a document that cannot be loaded
index.events.error_occurred += lambda sender, args: print(args.message)

# Creating a document object
document_key = url
document_loader = DocumentLoaderFromUrl(document_key, url, ".pdf")
document = Document.create_lazy(DocumentSourceKind.STREAM, document_key, document_loader)
documents = [
    document,
]

# Indexing the lazy-loaded document
options = IndexingOptions()
index.add(documents, options)

print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< /tabs >}}

## Indexing from FTP

The following example demonstrates how to index a document from FTP when lazy initialized. `urllib.request` downloads `ftp://` URLs too.

{{< tabs "example5" >}}
{{< tab "Python" >}}
```python
import datetime
import io
import urllib.request

from groupdocs.search import Index
from groupdocs.search.common import Document, DocumentSourceKind
from groupdocs.search.options import IndexingOptions


class DocumentLoaderFromFtp(object):
    def __init__(self, document_key, url, extension):
        self.document_key = document_key
        self.url = url
        self.extension = extension

    def load_document(self):
        # urllib reads ftp:// URLs as well; credentials can be given as ftp://user:password@host/path
        with urllib.request.urlopen(self.url) as response:
            memory_stream = io.BytesIO(response.read())
        document = Document.create_from_stream(self.document_key, datetime.datetime.now(), self.extension, memory_stream)
        return document

    def close_document(self):
        pass


url = "ftp://example.com/ExampleDocument.pdf"

# Creating an index
index = Index("./index")
# Reporting errors, for example, a document that cannot be loaded
index.events.error_occurred += lambda sender, args: print(args.message)

# Creating a document object
document_key = url
document_loader = DocumentLoaderFromFtp(document_key, url, ".pdf")
document = Document.create_lazy(DocumentSourceKind.STREAM, document_key, document_loader)
documents = [
    document,
]

# Indexing the lazy-loaded document
options = IndexingOptions()
index.add(documents, options)

print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< /tabs >}}

## Indexing from Amazon S3 Storage

The following example demonstrates how to index a document from Amazon S3 Storage when lazy initialized. The example uses the [boto3](https://pypi.org/project/boto3/) package (`pip install boto3`), which takes the AWS credentials from its usual configuration, for example, environment variables.

{{< tabs "example6" >}}
{{< tab "Python" >}}
```python
import datetime
import io

try:
    import boto3  # pip install boto3
except ImportError:
    boto3 = None

from groupdocs.search import Index
from groupdocs.search.common import Document, DocumentSourceKind
from groupdocs.search.options import IndexingOptions


class DocumentLoaderFromAmazon(object):
    def __init__(self, document_key, extension, storage_key):
        self.document_key = document_key
        self.extension = extension
        self.storage_key = storage_key

    def load_document(self):
        client = boto3.client("s3")
        bucket_name = "my-bucket"
        response = client.get_object(Bucket=bucket_name, Key=self.storage_key)
        memory_stream = io.BytesIO(response["Body"].read())
        document = Document.create_from_stream(self.document_key, datetime.datetime.now(), self.extension, memory_stream)
        return document

    def close_document(self):
        pass


key = "example.pdf"

# Creating an index
index = Index("./index")
# Reporting errors, for example, a document that cannot be loaded
index.events.error_occurred += lambda sender, args: print(args.message)

# Creating a document object
document_loader = DocumentLoaderFromAmazon("documentKey", ".pdf", key)
document = Document.create_lazy(DocumentSourceKind.STREAM, "documentKey", document_loader)
documents = [
    document,
]

# Indexing the lazy-loaded document
if boto3 is None:
    print("boto3 is not installed (pip install boto3), so the document is not indexed")
else:
    options = IndexingOptions()
    index.add(documents, options)

print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< /tabs >}}

## Indexing from Azure Blob Storage

The following example demonstrates how to index a document from Azure Blob Storage when lazy initialized. The example uses the [azure-storage-blob](https://pypi.org/project/azure-storage-blob/) package (`pip install azure-storage-blob`).

{{< tabs "example7" >}}
{{< tab "Python" >}}
```python
import datetime
import io

try:
    from azure.storage.blob import BlobClient  # pip install azure-storage-blob
except ImportError:
    BlobClient = None

from groupdocs.search import Index
from groupdocs.search.common import Document, DocumentSourceKind
from groupdocs.search.options import IndexingOptions


class DocumentLoaderFromAzure(object):
    def __init__(self, document_key, extension, blob_name):
        self.document_key = document_key
        self.extension = extension
        self.blob_name = blob_name

    def load_document(self):
        connection_string = "***"  # The connection string of the storage account
        container_name = "***"
        blob_client = BlobClient.from_connection_string(connection_string, container_name, self.blob_name)

        memory_stream = io.BytesIO(blob_client.download_blob().readall())

        document = Document.create_from_stream(self.document_key, datetime.datetime.now(), self.extension, memory_stream)
        return document

    def close_document(self):
        pass


blob_name = "example.pdf"

# Creating an index
index = Index("./index")
# Reporting errors, for example, a document that cannot be loaded
index.events.error_occurred += lambda sender, args: print(args.message)

# Creating a document object
document_loader = DocumentLoaderFromAzure("documentKey", ".pdf", blob_name)
document = Document.create_lazy(DocumentSourceKind.STREAM, "documentKey", document_loader)
documents = [
    document,
]

# Indexing the lazy-loaded document
if BlobClient is None:
    print("azure-storage-blob is not installed (pip install azure-storage-blob), so the document is not indexed")
else:
    options = IndexingOptions()
    index.add(documents, options)

print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
