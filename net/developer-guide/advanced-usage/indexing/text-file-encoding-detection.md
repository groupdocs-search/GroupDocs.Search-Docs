---
id: text-file-encoding-detection
url: search/net/text-file-encoding-detection
title: Text file encoding detection
weight: 21
description: "This article explains that how to detect encoding of a text file automatically."
keywords: detect encoding of a text file, detect encoding
productName: GroupDocs.Search for .NET
hideChildren: False
---
To automatically detect encoding of a text file, the [AutoDetectEncoding](https://reference.groupdocs.com/net/search/groupdocs.search/indexsettings/properties/autodetectencoding) property defined in the [IndexSettings](https://reference.groupdocs.com/net/search/groupdocs.search/indexsettings) class can be used. Setting this property to true allows to detect the following encodings:

*   UTF-32 LE,
*   UTF-32 BE,
*   UTF-16 LE,
*   UTF-16 BE,
*   UTF-8,
*   UTF-7,

By default, the encoding auto detection of text files is disabled. But in any case, the encoding of a text file can be set during indexing when the [FileIndexing](https://reference.groupdocs.com/net/search/groupdocs.search.events/eventhub/events/fileindexing) event is raised. If the encoding of a text file has not been detected or specified in the event arguments, then the default encoding, UTF-8, is used. Available encodings are presented in the [Encodings](https://reference.groupdocs.com/net/search/groupdocs.search.common/encodings) class. When the encoding of a text file is detected and used for indexing, it is saved in the index to use in such methods of [Index](https://reference.groupdocs.com/net/search/groupdocs.search/index) class like [Highlight](https://reference.groupdocs.com/net/search/groupdocs.search/index/methods/highlight/index) and [GetDocumentText](https://reference.groupdocs.com/net/search/groupdocs.search/index/methods/getdocumenttext/index).

The example below shows how to set encoding of a text during indexing.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.FileIndexing += (sender, args) =>
{
    if (args.DocumentFullPath.EndsWith(".txt", StringComparison.InvariantCultureIgnoreCase))
    {
        args.Encoding = Encodings.Windows_1253; // Setting encoding for each text file
    }
};
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
```

External tools, such as Utf.Unknown, can be used to determine the encoding of a text file during indexing.
Any encoding can be detected, including ANSI.

```
PM> NuGet\Install-Package UTF.Unknown
```

Below is an example of using the external library to determine the encoding of a text file.

**C#**

```csharp
index.Events.FileIndexing += (sender, args) =>
{
    byte[] data = File.ReadAllBytes(args.DocumentFullPath);
    UtfUnknown.DetectionResult result = UtfUnknown.CharsetDetector.DetectFromBytes(data);
    if (result.Detected != null)
    {
        Console.WriteLine("Encoding detected: " + result.Detected.EncodingName);
        args.Encoding = result.Detected.EncodingName;
    }
};
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
