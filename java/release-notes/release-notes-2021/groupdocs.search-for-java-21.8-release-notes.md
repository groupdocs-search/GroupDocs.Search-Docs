---
id: groupdocs-search-for-java-21-8-release-notes
url: search/java/groupdocs-search-for-java-21-8-release-notes
title: GroupDocs.Search for Java 21.8 Release Notes
weight: 10
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---

{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 21.8{{< /alert >}}

## Major Features

There are the following features, enhancements, and fixes in this release:

- Implement OCR support for indexing

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2564 | Implement OCR support for indexing | Feature |

## Public API and Backward Incompatible Changes

### Implement OCR support for indexing

This feature implements the ability to connect optical character recognition (OCR) on images inside individual image files, as well as on images embedded in documents.

##### Public API changes

Enumeration **ImageLocation** has been added to **com.groupdocs.search.options** package.  
Value **Separate** has been added to **com.groupdocs.search.options.ImageLocation** enumeration.  
Value **Embedded** has been added to **com.groupdocs.search.options.ImageLocation** enumeration.  
Value **ContainerItem** has been added to **com.groupdocs.search.options.ImageLocation** enumeration.

Interface **IOcrConnector** has been added to **com.groupdocs.search.options** package.  
Method **String recognize(OcrContext)** has been added to **com.groupdocs.search.options.IOcrConnector** interface.

Class **OcrIndexingOptions** has been added to **com.groupdocs.search.options** package.  
Method **boolean getEnabledForSeparateImages()** has been added to **com.groupdocs.search.options.OcrIndexingOptions** class.  
Method **void setEnabledForSeparateImages(boolean)** has been added to **com.groupdocs.search.options.OcrIndexingOptions** class.  
Method **boolean getEnabledForEmbeddedImages()** has been added to **com.groupdocs.search.options.OcrIndexingOptions** class.  
Method **void setEnabledForEmbeddedImages(boolean)** has been added to **com.groupdocs.search.options.OcrIndexingOptions** class.  
Method **IOcrConnector getOcrConnector()** has been added to **com.groupdocs.search.options.OcrIndexingOptions** class.  
Method **void setOcrConnector(IOcrConnector)** has been added to **com.groupdocs.search.options.OcrIndexingOptions** class.

Method **OcrIndexingOptions getOcrIndexingOptions()** has been added to **com.groupdocs.search.options.IndexingOptions** class.

Class **OcrContext** has been added to **com.groupdocs.search.options** package.  
Constructor **OcrContext(InputStream, ImageLocation, String)** has been added to **com.groupdocs.search.options.OcrContext** class.  
Method **InputStream getImageStream()** has been added to **com.groupdocs.search.options.OcrContext** class.  
Method **ImageLocation getImageLocation()** has been added to **com.groupdocs.search.options.OcrContext** class.  
Method **String getImageFileExtension()** has been added to **com.groupdocs.search.options.OcrContext** class.

Method **OcrIndexingOptions getOcrIndexingOptions()** has been added to **com.groupdocs.search.options.IndexingOptions** class.

Method **OcrIndexingOptions getOcrIndexingOptions()** has been added to **com.groupdocs.search.options.TextOptions** class.

Method **OcrIndexingOptions getOcrIndexingOptions()** has been added to **com.groupdocs.search.options.UpdateOptions** class.

Constant **String EmbeddedImageOcr** has been added to **com.groupdocs.search.options.CommonFieldNames** class.

##### Use cases

The following example demonstrates how to connect com.aspose.ocr package for text recognition.

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

