---
id: ocr-support
url: search/python-net/ocr-support
title: OCR support
weight: 21
description: "This article demonstrates the ability to connect an external module (library) for the recognition of printed text (optical character recognition, OCR) on images, either separate or embedded in documents."
keywords: optical character recognition, OCR, IOcrConnector, OcrIndexingOptions, recognize text in images, EasyOCR, Tesseract, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
OCR support means the ability to connect an external module (library) for the recognition of printed text (optical character recognition, OCR) on images, either separate or embedded in documents.

To connect OCR, you need to implement the [IOcrConnector](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/iocrconnector/) interface in the client code and assign the connector to the [ocr_connector](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/ocrindexingoptions/ocr_connector/) property of the [OcrIndexingOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/ocrindexingoptions/) object of the indexing options. The [recognize](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/iocrconnector/recognize/) method receives an [OcrContext](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/ocrcontext/) object, which provides the image stream (`context.image_stream.read()` returns the bytes of the image), the location of the image ([ImageLocation](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imagelocation/): a separate image file, an image embedded in a document or an image that is an item of a container) and the image file extension. The method returns the recognized text, or `None` if there is none. In Python, the connector is a plain class with a `recognize(self, context)` method; do not derive it from the `IOcrConnector` class. Since the interface has one method, a function that takes the context can be assigned as the connector too. The [OcrIndexingOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/ocrindexingoptions/) class also specifies for which images text recognition is enabled (see [Indexing options]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-options.md" >}})).

Python has no built-in OCR, so the connector calls an OCR package. The following example demonstrates how to implement the OCR connector using the [EasyOCR](https://pypi.org/project/easyocr/) package for text recognition in images (`pip install easyocr`). Without the package, the connector prints the images it receives and recognizes no text, so the example runs without it.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import ImageLocation, IndexingOptions

try:
    import easyocr  # pip install easyocr
except ImportError:
    easyocr = None


# Implementing the OCR connector that uses the EasyOCR package
class EasyOcrConnector(object):
    def __init__(self):
        self.reader = easyocr.Reader(["en"]) if easyocr is not None else None

    def recognize(self, context):
        extension = context.image_file_extension.lower()
        if context.image_location == ImageLocation.SEPARATE:
            if extension in (".gif", ".png", ".jpg", ".jpeg", ".bmp", ".tif", ".tiff", ""):
                return self.recognize_private(context)
            return None
        elif context.image_location in (ImageLocation.EMBEDDED, ImageLocation.CONTAINER_ITEM):
            return self.recognize_private(context)
        else:
            raise NotImplementedError("The image type is not supported: " + str(context.image_location))

    def recognize_private(self, context):
        image = context.image_stream.read()  # Reading the whole image stream
        if self.reader is None:
            # Without EasyOCR, the connector only reports the image it receives
            print("Image: %s, extension '%s', %d bytes"
                  % (context.image_location.name, context.image_file_extension, len(image)))
            return None
        return " ".join(self.reader.readtext(image, detail=0))


# Creating an index
index = Index("./index")

# Setting the OCR indexing options
options = IndexingOptions()
options.ocr_indexing_options.enabled_for_separate_images = True
options.ocr_indexing_options.enabled_for_embedded_images = True
options.ocr_indexing_options.ocr_connector = EasyOcrConnector()

# Indexing documents in a document folder
index.add("./documents", options)

# Searching in the index
result = index.search("Einstein")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/ocr-support/documents/Einstein.txt), [documents/catalogue-cards.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/ocr-support/documents/catalogue-cards.docx), [documents/catalogue-card.png](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/ocr-support/documents/catalogue-card.png)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "ocr-support.txt" >}}  
```text
Image: SEPARATE, extension '', 14241 bytes
Image: EMBEDDED, extension '', 14241 bytes
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/ocr-support/ocr_support/ocr-support.txt)
{{< /tab >}}
{{< /tabs >}}

The next example demonstrates a connector written as a function. Such a function can send the image to a cloud OCR service, for example [Aspose.OCR Cloud](https://products.aspose.cloud/ocr/), and return the recognized text; the example only prints what it receives.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import IndexingOptions


# Implementing the OCR connector as a function
def recognize(context):
    image = context.image_stream.read()
    # Send the image to an OCR service here, for example, Aspose.OCR Cloud,
    # and return the text it recognizes
    print("Image to recognize: %s, extension '%s', %d bytes"
          % (context.image_location.name, context.image_file_extension, len(image)))
    return None


# Creating an index
index = Index("./index")

# Setting the OCR indexing options
options = IndexingOptions()
options.ocr_indexing_options.enabled_for_separate_images = True
options.ocr_indexing_options.ocr_connector = recognize

# Indexing documents in a document folder
index.add("./documents", options)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/ocr-support/documents/Einstein.txt), [documents/catalogue-cards.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/ocr-support/documents/catalogue-cards.docx), [documents/catalogue-card.png](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/ocr-support/documents/catalogue-card.png)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "ocr-support-2.txt" >}}  
```text
Image to recognize: SEPARATE, extension '', 14241 bytes
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/ocr-support/ocr_support_2/ocr-support-2.txt)
{{< /tab >}}
{{< /tabs >}}

And the following example demonstrates how to implement the OCR connector using Tesseract through the [pytesseract](https://pypi.org/project/pytesseract/) package (`pip install pytesseract pillow`), which also needs the [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) engine with the English language data.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import io

from groupdocs.search import Index
from groupdocs.search.options import IndexingOptions

try:
    # pip install pytesseract pillow
    # The Tesseract OCR engine with the English language data must be installed too
    import pytesseract
    from PIL import Image
except ImportError:
    pytesseract = None


# Implementing the OCR connector that uses Tesseract
class TesseractOcrConnector(object):
    def recognize(self, context):
        buffer = context.image_stream.read()  # Reading the whole image stream
        if pytesseract is None:
            print("pytesseract is not installed: an image of %d bytes is not recognized" % len(buffer))
            return None
        with Image.open(io.BytesIO(buffer)) as image:
            recognized_text = pytesseract.image_to_string(image, lang="eng")
        return recognized_text


# Creating an index
index = Index("./index")

# Setting the OCR indexing options
options = IndexingOptions()
options.ocr_indexing_options.enabled_for_separate_images = True
options.ocr_indexing_options.ocr_connector = TesseractOcrConnector()

# Indexing documents in a document folder
index.add("./documents", options)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/ocr-support/documents/Einstein.txt), [documents/catalogue-cards.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/ocr-support/documents/catalogue-cards.docx), [documents/catalogue-card.png](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/ocr-support/documents/catalogue-card.png)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "ocr-support-3.txt" >}}  
```text
pytesseract is not installed: an image of 14241 bytes is not recognized
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/ocr-support/ocr_support_3/ocr-support-3.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
