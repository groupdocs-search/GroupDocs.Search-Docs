---
id: custom-text-extractors
url: search/net/custom-text-extractors
title: Custom text extractors
weight: 4
description: "This article explains how to implement a custom text extractor to index documents of any format, including container documents."
keywords: custom text extractor, IFieldExtractor, IContainerItemExtractor, unsupported formats, CustomExtractors
productName: GroupDocs.Search for .NET
hideChildren: False
---
GroupDocs.Search supports indexing of many document formats. In addition, you can implement support for any other format, or replace the built-in extraction for a supported format, with a custom text extractor.

A custom text extractor is a class implementing the [IFieldExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.common/ifieldextractor/) interface. The [Extensions](https://reference.groupdocs.com/search/net/groupdocs.search.common/ifieldextractor/extensions/) property returns the file extensions the extractor is used for, and the two [GetFields](https://reference.groupdocs.com/search/net/groupdocs.search.common/ifieldextractor/getfields/) methods extract the fields of a document from a file or from a stream. The method that takes a stream is used for documents added from streams.

The following example demonstrates how to implement a custom text extractor.

**C#**

```csharp
public class LogExtractor : IFieldExtractor
{
    private readonly string[] extensions = new string[] { ".log" };
 
    public string[] Extensions
    {
        get { return extensions; }
    }
 
    public DocumentField[] GetFields(string filePath)
    {
        FileInfo fileInfo = new FileInfo(filePath);
        DocumentField[] fields = new DocumentField[]
        {
            new DocumentField("FileName", fileInfo.FullName),
            new DocumentField("CreationDate", fileInfo.CreationTime.ToString(CultureInfo.InvariantCulture)),
            new DocumentField("Content", ExtractContent(filePath)),
        };
        return fields;
    }
 
    public DocumentField[] GetFields(Stream stream)
    {
        // The stream belongs to the caller, so it is not closed here
        using (StreamReader reader = new StreamReader(stream, Encoding.UTF8, true, 4096, true))
        {
            DocumentField[] fields = new DocumentField[]
            {
                new DocumentField("Content", reader.ReadToEnd()),
            };
            return fields;
        }
    }
 
    private string ExtractContent(string filePath)
    {
        string text = File.ReadAllText(filePath);
        return text;
    }
}
```

To use a custom extractor, add it to the [CustomExtractors](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/customextractors/) collection of the index settings. A custom extractor takes precedence over the built-in extractor for the same extension. Custom extractors are not saved in an index, so they must be added each time the index is created or opened. The next example shows how to use the custom extractor for indexing.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\"; // Specify path to the index folder
string documentsFolder = @"c:\MyDocuments\"; // Specify path to a folder containing documents to search
 
IndexSettings settings = new IndexSettings();
settings.CustomExtractors.Add(new LogExtractor()); // Adding custom text extractor to the index settings
 
Index index = new Index(indexFolder, settings); // Creating or loading an index
 
index.Add(documentsFolder); // Indexing documents from the specified folder
```

A custom extractor can also be specified for an individual document in the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event handler through the [CustomExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.events/fileindexingeventargs/customextractor/) property of the event arguments (see [Search index events]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}})), and for [separate data extraction]({{< ref "search/net/developer-guide/advanced-usage/indexing/separate-data-extraction.md" >}}) through the [ExtractionOptions.CustomExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.options/extractionoptions/customextractor/) property.

## Custom extractors for container documents

To index a container document (for example, an archive) whose items are indexed as separate items of the document, implement the [IContainerItemExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.common/icontaineritemextractor/) interface. In addition to the members of [IFieldExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.common/ifieldextractor/), this interface contains:

*   The [FormatFamily](https://reference.groupdocs.com/search/net/groupdocs.search.common/icontaineritemextractor/formatfamily/) property, which returns the format family of the container document.
*   Two [GetContainerItems](https://reference.groupdocs.com/search/net/groupdocs.search.common/icontaineritemextractor/getcontaineritems/) methods, which extract the items of a container document from a file or from a stream and return them as an array of [ExtractedItemInfo](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractediteminfo/) objects.

Each [ExtractedItemInfo](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractediteminfo/) object describes one item: its [FormatFamily](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractediteminfo/formatfamily/), the extracted [Fields](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractediteminfo/fields/), the [ItemInfo](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractediteminfo/iteminfo/) string that identifies the item in the container, and the [InnerPath](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractediteminfo/innerpath/) of the item. Items with an empty [ItemInfo](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractediteminfo/iteminfo/) value are not indexed. The fields returned by [GetFields](https://reference.groupdocs.com/search/net/groupdocs.search.common/ifieldextractor/getfields/) are indexed as the fields of the container document itself. The indexed items of a container document can be obtained with the [GetIndexedDocumentItems](https://reference.groupdocs.com/search/net/groupdocs.search/index/getindexeddocumentitems/) method.

The following example demonstrates a custom extractor that indexes each line of a text file as a separate item.

**C#**

```csharp
public class LineContainerExtractor : IContainerItemExtractor
{
    private readonly string[] extensions = new string[] { ".lines" };
 
    public string[] Extensions
    {
        get { return extensions; }
    }
 
    public FormatFamily FormatFamily
    {
        get { return FormatFamily.Archive; }
    }
 
    public DocumentField[] GetFields(string filePath)
    {
        // The container document itself has only the file name field
        return new DocumentField[] { new DocumentField("FileName", Path.GetFileName(filePath)) };
    }
 
    public DocumentField[] GetFields(Stream stream)
    {
        return new DocumentField[0];
    }
 
    public ExtractedItemInfo[] GetContainerItems(string filePath)
    {
        return CreateItems(File.ReadAllLines(filePath));
    }
 
    public ExtractedItemInfo[] GetContainerItems(Stream stream)
    {
        List<string> lines = new List<string>();
        using (StreamReader reader = new StreamReader(stream, Encoding.UTF8, true, 4096, true))
        {
            string line;
            while ((line = reader.ReadLine()) != null)
            {
                lines.Add(line);
            }
        }
        return CreateItems(lines.ToArray());
    }
 
    private static ExtractedItemInfo[] CreateItems(string[] lines)
    {
        ExtractedItemInfo[] items = new ExtractedItemInfo[lines.Length];
        for (int i = 0; i < lines.Length; i++)
        {
            DocumentField[] fields = new DocumentField[] { new DocumentField("Content", lines[i]) };
            string itemInfo = "Line " + (i + 1).ToString(CultureInfo.InvariantCulture);
            items[i] = new ExtractedItemInfo(FormatFamily.WordProcessing, fields, itemInfo, string.Empty);
        }
        return items;
    }
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
