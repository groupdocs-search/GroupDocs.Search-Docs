---
id: ocr-support
url: search/java/ocr-support
title: OCR support
weight: 18
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
OCR support means the ability to connect an external module (library) for the recognition of printed text (optical character recognition, OCR) on images, either separate or embedded in documents.

To connect OCR, you need to implement the IOcrConnector interface in the client code.

The following example demonstrates how to implement the OCR connector using com.aspose.ocr library for text recognition in images.

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Setting the OCR indexing options
IndexingOptions options = new IndexingOptions();
options.getOcrIndexingOptions().setEnabledForSeparateImages(true);
options.getOcrIndexingOptions().setEnabledForEmbeddedImages(true);
options.getOcrIndexingOptions().setOcrConnector(new OcrConnector());

// Indexing documents in a document folder
index.add(documentFolder, options);

// Searching in the index
SearchResult result = index.search("Einstein");

...

// Implementing the OCR connector that uses com.aspose.ocr library
public static class OcrConnector implements IOcrConnector {
    public OcrConnector() {
    }

    @Override
    public final String recognize(OcrContext context) {
        if (null == context.getImageLocation()) {
            throw new NotSupportedException("The image type is not supported: " + context.getImageLocation());
        } else {
            switch (context.getImageLocation()) {
            case Separate:
            case Embedded:
            case ContainerItem:
                return recognizePrivate(context);
            default:
                throw new NotSupportedException("The image type is not supported: " + context.getImageLocation());
            }
        }
    }

    private String recognizePrivate(OcrContext context) {
        try {
            java.awt.image.BufferedImage image = javax.imageio.ImageIO.read(context.getImageStream());
            com.aspose.ocr.AsposeOCR asposeOcr = new com.aspose.ocr.AsposeOCR();
            String result = asposeOcr.RecognizePage(image);
            return result;
        } catch (java.io.IOException ex) {
            throw new RuntimeException(ex);
        }
    }
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

* [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

* [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
