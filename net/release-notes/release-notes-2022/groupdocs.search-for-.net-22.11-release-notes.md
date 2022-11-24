---
id: groupdocs-search-for-net-22-11-release-notes
url: search/net/groupdocs-search-for-net-22-11-release-notes
title: GroupDocs.Search for .NET 22.11 Release Notes
weight: 8
description: ""
keywords: 
productName: GroupDocs.Search for .NET
hideChildren: False
---

{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for .NET 22.11{{< /alert >}}

## Major Features

There are the following features, enhancements, and fixes in this release:

- Implement text extraction for each field separately
- Implement text extraction in plain text format

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2759 | Implement text extraction for each field separately | Enhancement |
| SEARCHNET-2783 | Implement text extraction in plain text format | Enhancement |

## Public API and Backward Incompatible Changes

### Implement text extraction for each field separately

This enhancement adds a new StructureOutputAdapter class that collects the extracted text separately for each document field. For more information on output adapters, see the [Output adapters](https://docs.groupdocs.com/search/net/output-adapters/) documentation page.

##### Public API changes

Class **StructureOutputAdapter** has been added to **GroupDocs.Search.Common** namespace.  
Method **GroupDocs.Search.Common.DocumentField[] GetResult()** has been added to **GroupDocs.Search.Common.StructureOutputAdapter** class.  
Constructor **StructureOutputAdapter(GroupDocs.Search.Options.OutputFormat)** has been added to **GroupDocs.Search.Common.StructureOutputAdapter** class.

##### Use cases

The following example demonstrates how to output the extractracted text with different output adapters.

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index settings instance
IndexSettings settings = new IndexSettings();
settings.TextStorageSettings = new TextStorageSettings(Compression.High); // Enabling the storage of extracted text in the index

// Creating an index in the specified folder
Index index = new Index(indexFolder, settings);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Getting list of indexed documents
DocumentInfo[] documents = index.GetIndexedDocuments();

// Getting a document text
if (documents.Length > 0)
{
    DocumentInfo document = documents[0];

    // Output to a file
    FileOutputAdapter fileOutputAdapter = new FileOutputAdapter(OutputFormat.Html, @"C:\Text.html");
    index.GetDocumentText(document, fileOutputAdapter);

    // Output to a stream
    using (Stream stream = new MemoryStream())
    {
        StreamOutputAdapter streamOutputAdapter = new StreamOutputAdapter(OutputFormat.Html, stream);
        index.GetDocumentText(document, streamOutputAdapter);
    }

    // Output to a string
    StringOutputAdapter stringOutputAdapter = new StringOutputAdapter(OutputFormat.Html);
    index.GetDocumentText(document, stringOutputAdapter);
    string htmlText = stringOutputAdapter.GetResult();
    Console.WriteLine(htmlText);

    // Output to a structure
    StructureOutputAdapter structureOutputAdapter = new StructureOutputAdapter(OutputFormat.PlainText);
    index.GetDocumentText(document, structureOutputAdapter);
    DocumentField[] fields = structureOutputAdapter.GetResult();
    Console.WriteLine(document.ToString());
    for (int i = 0; i < fields.Length; i++)
    {
        DocumentField field = fields[i];
        Console.WriteLine("\t" + field.Name);
    }
}
```

### Implement text extraction in plain text format

This enhancement implements the ability to output the text of the document in plain text format, both when obtaining the text of the document from the index and when highlighting the found words in the text. For more information, see the [Getting indexed documents](https://docs.groupdocs.com/search/net/getting-indexed-documents/) documentation page and the [Highlighting search results](https://docs.groupdocs.com/search/net/highlighting-search-results/) documentation page.

##### Public API changes

Enum **OutputFormat** has been added to **GroupDocs.Search.Options** namespace.  
Field **GroupDocs.Search.Options.OutputFormat Html** has been added to **GroupDocs.Search.Options.OutputFormat** enum.  
Field **GroupDocs.Search.Options.OutputFormat PlainText** has been added to **GroupDocs.Search.Options.OutputFormat** enum.

Class **DocumentHighlighter** has been added to **GroupDocs.Search.Highlighters** namespace.  
Constructor **DocumentHighlighter(GroupDocs.Search.Common.OutputAdapter)** has been added to **GroupDocs.Search.Highlighters.DocumentHighlighter** class.  
Property **GroupDocs.Search.Common.OutputAdapter OutputAdapter** has been added to **GroupDocs.Search.Highlighters.DocumentHighlighter** class.

Class **FragmentHighlighter** has been added to **GroupDocs.Search.Highlighters** namespace.  
Constructor **FragmentHighlighter(GroupDocs.Search.Options.OutputFormat)** has been added to **GroupDocs.Search.Highlighters.FragmentHighlighter** class.  
Method **GroupDocs.Search.Common.FragmentContainer[] GetResult()** has been added to **GroupDocs.Search.Highlighters.FragmentHighlighter** class.

Property **System.String TermHighlightEndTag** has been added to **GroupDocs.Search.Options.HighlightOptions** class.  
Property **System.String TermHighlightStartTag** has been added to **GroupDocs.Search.Options.HighlightOptions** class.

Property **GroupDocs.Search.Options.OutputFormat OutputFormat** has been added to **GroupDocs.Search.Common.ResultBuilderFactory** class.

Constructor **FileOutputAdapter(GroupDocs.Search.Options.OutputFormat, System.String)** has been added to **GroupDocs.Search.Common.FileOutputAdapter** class.  
Constructor **StreamOutputAdapter(GroupDocs.Search.Options.OutputFormat, System.IO.Stream)** has been added to **GroupDocs.Search.Common.StreamOutputAdapter** class.  
Constructor **StringOutputAdapter(GroupDocs.Search.Options.OutputFormat)** has been added to **GroupDocs.Search.Common.StringOutputAdapter** class.

##### Use cases

The following example demonstrates how to highlight search results in plain text format.

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";

// Creating an index
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentFolder);

// Search for the word 'Universe'
SearchResult result = index.Search("Universe");

// Highlighting occurrences in the text
if (result.DocumentCount > 0)
{
    FoundDocument document = result.GetFoundDocument(0); // Getting the first found document
    StructureOutputAdapter outputAdapter = new StructureOutputAdapter(OutputFormat.PlainText); // Creating the output adapter
    Highlighter highlighter = new DocumentHighlighter(outputAdapter); // Creating the highlighter instance
    HighlightOptions options = new HighlightOptions(); // Creating the highlight options
    options.TermHighlightStartTag = "<Term>"; // Setting the start tag for the found word
    options.TermHighlightEndTag = "</Term>"; // Setting the end tag for the found word
    index.Highlight(document, highlighter, options); // Generating plain text with highlighted occurrences

    DocumentField[] fields = outputAdapter.GetResult();
    Console.WriteLine(document.ToString());
    for (int i = 0; i < fields.Length; i++)
    {
        // Printing field names of the found document
        DocumentField field = fields[i];
        Console.WriteLine("\t" + field.Name);
    }
}
```
