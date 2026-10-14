---
id: get-supported-file-formats
url: search/net/get-supported-file-formats
title: Get supported file formats
weight: 5
description: "This article describes how to obtain the list of file types supported by GroupDocs.Search for .NET."
keywords: supported file types, supported formats, FileType, GetSupportedFileTypes
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [GetSupportedFileTypes](https://reference.groupdocs.com/search/net/groupdocs.search.results/filetype/getsupportedfiletypes/) method of the [FileType](https://reference.groupdocs.com/search/net/groupdocs.search.results/filetype/) class is used to obtain a list of supported file types.

The returned collection also contains the [FileType.Unknown](https://reference.groupdocs.com/search/net/groupdocs.search.results/filetype/unknown/) value, which has an empty extension. An example of obtaining a list of supported file types is presented below. The example uses LINQ methods, so it requires the *System.Linq* namespace.

**C#**

```csharp
IEnumerable<FileType> supportedFileTypes = FileType
    .GetSupportedFileTypes()
    .Where(ft => ft != FileType.Unknown) // Skipping the unknown file type
    .OrderBy(ft => ft.Extension);
 
foreach (FileType fileType in supportedFileTypes)
{
    Console.WriteLine(fileType.Extension.PadRight(8) + " - " + fileType.Description);
}
```

## More resources

### Advanced usage topics

To learn more about search features and get familiar how to enhance your search solution, please refer to the [advanced usage section]({{< ref "search/net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
