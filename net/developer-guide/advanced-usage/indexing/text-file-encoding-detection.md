---
id: text-file-encoding-detection
url: search/net/text-file-encoding-detection
title: Text file encoding detection
weight: 25
description: "This article explains how to detect or specify the encoding of text files during indexing."
keywords: detect encoding of a text file, text file encoding, AutoDetectEncoding, Encodings, HTML encoding
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

Since version 26.11, with the [extraction in a separate process]({{< ref "search/net/developer-guide/advanced-usage/indexing/extraction-in-separate-process.md" >}}), the encoding of a document is detected in the extraction process, and the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event gets it as before; before, the main process opened the documents to detect their encodings, so a failure on a document was not isolated in the extraction process. A document from a structure or from extracted data has no encoding to detect: since version 26.11, it is no longer reported as an error when the encodings are detected.

On .NET Core 3.1, .NET 5 and later, the code pages of Windows, such as windows-1251 or windows-1252, are not a part of the platform. Since version 26.11, the library registers the provider of the code pages of .NET ([CodePagesEncodingProvider](https://learn.microsoft.com/dotnet/api/system.text.codepagesencodingprovider)) for the process when it first reads a text document, so a text in such an encoding is read whether it is set by the options, by the FileIndexing event or by the detection. Before, such a text failed with an error unless another library had registered the provider. Note that after the registration, [Encoding.GetEncoding](https://learn.microsoft.com/dotnet/api/system.text.encoding.getencoding) of the application gets the code pages of Windows as well, and on Windows, Encoding.GetEncoding(0) gives the code page of the system instead of UTF-8. On .NET Framework, the code pages are a part of the platform.

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

## Encoding of HTML documents

Since version 26.11, an HTML document (.html, .htm, .xhtml) is read in the first of the following encodings:

*   the encoding of its byte order mark;
*   the encoding that the [Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/encoding/) property of the indexing options or the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event sets for it;
*   the charset that it declares in a meta tag in its first 1024 bytes (`<meta charset="...">` or `<meta http-equiv="Content-Type" content="text/html; charset=...">`); a declared charset that is not the name of an encoding is passed over;
*   UTF-8.

The detection of the encodings ([AutoDetectEncoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/autodetectencoding/)) gives no encoding of an HTML document, so its declaration is taken. The encoding that the options or the event set is saved in the index, as the encoding of a text file, and the highlighting reads the document in it. Before, an HTML document was read in the encoding of the Encoding property only, UTF-8 by default, so a page in another encoding without a byte order mark was indexed with wrong letters: index such documents again. The HTML items of archives and emails are extracted as before.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
