---
id: ocr-support
url: search/net/ocr-support
title: OCR support
weight: 18
description: ""
keywords: 
productName: GroupDocs.Search for .NET
hideChildren: False
---
OCR support means the ability to connect an external module (library) for the recognition of printed text (optical character recognition, OCR) on images, either separate or embedded in documents.

To connect OCR, you need to implement the IOcrConnector interface in the client code.

The following example demonstrates how to implement the OCR connector using Aspose.OCR library for text recognition in images.

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
        MemoryStream memoryStream = new MemoryStream((int)context.ImageStream.Length);
        context.ImageStream.CopyTo(memoryStream);

        AsposeOcr asposeOcr = new AsposeOcr();
        string result = asposeOcr.RecognizeImage(memoryStream);
        return result;
    }
}
```

The next example demonstrates how to implement the OCR connector using Aspose Cloud OCR API.

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
        var buffer = new byte[context.ImageStream.Length];
        context.ImageStream.Read(buffer, 0, buffer.Length);

        var path = Path.GetDirectoryName(Assembly.GetExecutingAssembly().CodeBase);
        path = Path.Combine(path, "tessdata");
        path = path.Replace("file:\\", "");

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
