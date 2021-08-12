---
id: groupdocs-search-for-net-21-8-release-notes
url: search/net/groupdocs-search-for-net-21-8-release-notes
title: GroupDocs.Search for .NET 21.8 Release Notes
weight: 3
description: ""
keywords: 
productName: GroupDocs.Search for .NET
hideChildren: False
---

{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for .NET 21.8{{< /alert >}}

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

Enumeration **ImageLocation** has been added to **GroupDocs.Search.Options** namespace.  
Value **Separate** has been added to **GroupDocs.Search.Options.ImageLocation** enumeration.  
Value **Embedded** has been added to **GroupDocs.Search.Options.ImageLocation** enumeration.  
Value **ContainerItem** has been added to **GroupDocs.Search.Options.ImageLocation** enumeration.

Interface **IOcrConnector** has been added to **GroupDocs.Search.Options** namespace.  
Method **string Recognize(OcrContext)** has been added to **GroupDocs.Search.Options.IOcrConnector** interface.

Class **OcrIndexingOptions** has been added to **GroupDocs.Search.Options** namespace.  
Property **bool EnabledForSeparateImages** has been added to **GroupDocs.Search.Options.OcrIndexingOptions** class.  
Property **bool EnabledForEmbeddedImages** has been added to **GroupDocs.Search.Options.OcrIndexingOptions** class.  
Property **IOcrConnector OcrConnector** has been added to **GroupDocs.Search.Options.OcrIndexingOptions** class.

Property **OcrIndexingOptions OcrIndexingOptions** has been added to **GroupDocs.Search.Options.IndexingOptions** class.

Class **OcrContext** has been added to **GroupDocs.Search.Options** namespace.  
Property **Stream ImageStream** has been added to **GroupDocs.Search.Options.OcrContext** class.  
Property **ImageLocation ImageLocation** has been added to **GroupDocs.Search.Options.OcrContext** class.  
Property **string ImageFileExtension** has been added to **GroupDocs.Search.Options.OcrContext** class.

Property **OcrIndexingOptions OcrIndexingOptions** has been added to **GroupDocs.Search.Options.IndexingOptions** class.

Property **OcrIndexingOptions OcrIndexingOptions** has been added to **GroupDocs.Search.Options.TextOptions** class.

Property **OcrIndexingOptions OcrIndexingOptions** has been added to **GroupDocs.Search.Options.UpdateOptions** class.

Constant **string EmbeddedImageOcr** has been added to **GroupDocs.Search.Options.CommonFieldNames** class.

##### Use cases

The following example demonstrates how to connect Aspose.OCR library for text recognition.

```csharp
string indexFolder = @"c:\MyIndex";
string documentFolder = @"c:\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Setting the OCR indexing options
IndexingOptions options = new IndexingOptions();
options.OcrIndexingOptions.EnabledForSeparateImages = true;
options.OcrIndexingOptions.EnabledForEmbeddedImages = true;
options.OcrIndexingOptions.OcrConnector = new OcrConnector();

// Indexing documents in a document folder
index.Add(documentFolder, options);

// Searching in the index
SearchResult result = index.Search("Einstein");

...

// Implementing the OCR connector that uses Aspose.OCR library
public class OcrConnector : IOcrConnector
{
    public OcrConnector()
    {
    }

    public string Recognize(OcrContext context)
    {
        string extension = context.ImageFileExtension.ToLowerInvariant();
        if (context.ImageLocation == ImageLocation.Separate)
        {
            switch (extension)
            {
                case ".gif":
                case ".png":
                case ".jpg":
                case ".jpeg":
                case ".bmp":
                case ".tif":
                case ".tiff":
                case "":
                    return RecognizePrivate(context);

                default:
                    return null;
            }
        }
        else if (context.ImageLocation == ImageLocation.Embedded ||
            context.ImageLocation == ImageLocation.ContainerItem)
        {
            return RecognizePrivate(context);
        }
        else
        {
            throw new NotSupportedException("The image type is not supported: " + context.ImageLocation);
        }
    }

    private string RecognizePrivate(OcrContext context)
    {
        MemoryStream memoryStream = new MemoryStream((int)context.ImageStream.Length);
        context.ImageStream.CopyTo(memoryStream);

        AsposeOcr asposeOcr = new AsposeOcr();
        string result = asposeOcr.RecognizeImage(memoryStream);
        return result;
    }
}
```
