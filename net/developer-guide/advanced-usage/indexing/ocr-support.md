---
id: ocr-support
url: search/net/ocr-support
title: OCR support
weight: 21
description: "This article demonstrates the ability to connect an external module (library) for the recognition of printed text (optical character recognition, OCR) on images, either separate or embedded in documents."
keywords: optical character recognition, OCR, IOcrConnector, OcrIndexingOptions, recognize text in images
productName: GroupDocs.Search for .NET
hideChildren: False
---
OCR support means the ability to connect an external module (library) for the recognition of printed text (optical character recognition, OCR) on images, either separate or embedded in documents.

To connect OCR, you need to implement the [IOcrConnector](https://reference.groupdocs.com/search/net/groupdocs.search.options/iocrconnector/) interface in the client code and assign the connector to the [OcrConnector](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions/ocrconnector/) property of the [OcrIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions/) object of the indexing options. The [Recognize](https://reference.groupdocs.com/search/net/groupdocs.search.options/iocrconnector/recognize/) method receives an [OcrContext](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrcontext/) object, which provides the image stream, the location of the image ([ImageLocation](https://reference.groupdocs.com/search/net/groupdocs.search.options/imagelocation/): a separate image file, an image embedded in a document or an image that is an item of a container) and the image file extension. The method returns the recognized text. The [OcrIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/ocrindexingoptions/) class also specifies for which images text recognition is enabled (see [Indexing options]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-options.md" >}})).

The following example demonstrates how to implement the OCR connector using Aspose.OCR library for text recognition in images.

**C#**

```csharp
string indexFolder = @"c:\MyIndex";
string documentFolder = @"c:\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Setting the OCR indexing options
IndexingOptions options = new IndexingOptions();
options.OcrIndexingOptions.EnabledForSeparateImages = true;
options.OcrIndexingOptions.EnabledForEmbeddedImages = true;
options.OcrIndexingOptions.OcrConnector = new AsposeOcrConnector();

// Indexing documents in a document folder
index.Add(documentFolder, options);

// Searching in the index
SearchResult result = index.Search("Einstein");

...

// Implementing the OCR connector that uses Aspose.OCR library
// You need to install the following package:
// https://www.nuget.org/packages/Aspose.OCR/
public class AsposeOcrConnector : IOcrConnector
{
    public AsposeOcrConnector()
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
        MemoryStream memoryStream = new MemoryStream();
        context.ImageStream.CopyTo(memoryStream);
        memoryStream.Position = 0; // Rewinding the stream before recognition

        AsposeOcr asposeOcr = new AsposeOcr();
        string result = asposeOcr.RecognizeImage(memoryStream);
        return result;
    }
}
```

The next example demonstrates how to implement the OCR connector using Aspose Cloud OCR API.

**C#**

```csharp
// Implementing the OCR connector that uses Aspose Cloud OCR
// The full API for Aspose Cloud OCR for .NET you can find in the repository:
// https://github.com/aspose-ocr-cloud/aspose-ocr-cloud-dotnet
// Sid and key you can get after free registration at
// https://dashboard.aspose.cloud/applications
public class AsposeCloudOcrConnector : IOcrConnector
{
    private readonly Configuration _configuration;

    public AsposeCloudOcrConnector()
    {
        _configuration = new Configuration();
        _configuration.AppSid = "...";
        _configuration.AppKey = "...";
    }

    public string Recognize(OcrContext context)
    {
        OcrApi api = new OcrApi(_configuration);
        var request = new PostOcrFromUrlOrContentRequest(context.ImageStream);
        OCRResponse response = api.PostOcrFromUrlOrContent(request);
        return response.Text;
    }
}
```

And the following example demonstrates how to implement the OCR connector using Tesseract.

**C#**

```csharp
// Implementing the OCR connector that uses Tesseract
// You need to install the following packages:
// https://www.nuget.org/packages/Tesseract/
// https://www.nuget.org/packages/Tesseract.Data.English/
public class TesseractOcrConnector : IOcrConnector
{
    public TesseractOcrConnector()
    {
    }

    public string Recognize(OcrContext context)
    {
        byte[] buffer;
        using (MemoryStream memoryStream = new MemoryStream())
        {
            context.ImageStream.CopyTo(memoryStream); // Reading the whole image stream
            buffer = memoryStream.ToArray();
        }

        // The language data are expected in the tessdata folder of the application folder
        string path = Path.Combine(AppContext.BaseDirectory, "tessdata");

        using (var engine = new TesseractEngine(path, "eng", EngineMode.Default))
        using (Pix img = Pix.LoadFromMemory(buffer))
        using (Page recognizedPage = engine.Process(img))
        {
            string recognizedText = recognizedPage.GetText();
            return recognizedText;
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
