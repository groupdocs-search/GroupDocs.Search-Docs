---
id: text-file-encoding-detection
url: search/net/text-file-encoding-detection
title: Text file encoding detection
weight: 25
description: "This article explains how to detect or specify the encoding of text files during indexing."
keywords: detect encoding of a text file, text file encoding, AutoDetectEncoding, Encodings
productName: GroupDocs.Search for .NET
hideChildren: False
---
To automatically detect the encoding of a text file, use the [AutoDetectEncoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/autodetectencoding/) property of the [IndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/) class, which is inherited from the [BaseOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/) class and is also available in the [UpdateOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/updateoptions/) class. Setting this property to true allows you to detect the following encodings:

*   UTF-32 LE;
*   UTF-32 BE;
*   UTF-16 LE;
*   UTF-16 BE;
*   UTF-8;
*   UTF-7.

By default, the encoding auto detection of text files is disabled. But in any case, the encoding of a text file can be set during indexing when the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event is raised. If the encoding of a text file has not been detected or specified in the event arguments, then the default encoding is used: the value of the [Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/encoding/) property of the indexing options or, if it is null, UTF-8. Available encodings are presented in the [Encodings](https://reference.groupdocs.com/search/net/groupdocs.search.common/encodings/) class. When the encoding of a text file is detected and used for indexing, it is saved in the index to use in such methods of [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class like [Highlight](https://reference.groupdocs.com/search/net/groupdocs.search/index/highlight/) and [GetDocumentText](https://reference.groupdocs.com/search/net/groupdocs.search/index/getdocumenttext/).

The example below shows how to set the encoding of text files during indexing.

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

```powershell
PM> NuGet\Install-Package UTF.Unknown
```

Below is an example of using the external library to determine the encoding of a text file.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index
Index index = new Index(indexFolder);

// Subscribing to the event
index.Events.FileIndexing += (sender, args) =>
{
    if (args.DocumentFullPath != null &&
        args.DocumentFullPath.EndsWith(".txt", StringComparison.InvariantCultureIgnoreCase))
    {
        byte[] data = File.ReadAllBytes(args.DocumentFullPath);
        UtfUnknown.DetectionResult result = UtfUnknown.CharsetDetector.DetectFromBytes(data);
        if (result.Detected != null)
        {
            Console.WriteLine("Encoding detected: " + result.Detected.EncodingName);
            args.Encoding = result.Detected.EncodingName;
        }
    }
};

// Indexing documents from the specified folder
index.Add(documentsFolder);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
