---
id: highlighting-search-results
url: search/net/highlighting-search-results
title: Highlighting search results
weight: 8
description: "This article gives knowledge on how to highlight search results in the text of a document."
keywords: highlight search results
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page describes how to highlight search results in the text of a document.

## Hit highlighting in the text of entire document

After performing a search, occurrences of found words and phrases for a particular document can be highlighted in the text of the document using the [Highlight](https://apireference.groupdocs.com/net/search/groupdocs.search/index/methods/highlight/index) method of the [Index](https://apireference.groupdocs.com/net/search/groupdocs.search/index) class. To do this, a highlighter of the corresponding type must be passed as an argument to the method.

The Index class also represents an overload of the [Highlight](https://apireference.groupdocs.com/net/search/groupdocs.search/index/methods/highlight/index) method, which takes an object of the [HighlightOptions](https://apireference.groupdocs.com/net/search/groupdocs.search.options/highlightoptions) class as an argument. The [HighlightOptions](https://apireference.groupdocs.com/net/search/groupdocs.search.options/highlightoptions) class contains properties for setting the following options:

*   [CustomExtractor](https://apireference.groupdocs.com/net/search/groupdocs.search.options/textoptions/properties/customextractor) is an extractor used during indexing, it is necessary if the text of the document was not saved in the index.
*   [AdditionalFields](https://apireference.groupdocs.com/net/search/groupdocs.search.options/textoptions/properties/additionalfields) are additional document fields added during document indexing which are also necessary if the document text was not saved in the index.
*   [Cancellation](https://apireference.groupdocs.com/net/search/groupdocs.search.options/textoptions/properties/cancellation) is an object used to cancel the operation.
*   [GenerateHead](https://apireference.groupdocs.com/net/search/groupdocs.search.options/textoptions/properties/generatehead) is a flag to specify whether the Head tag is generated in the output HTML.
*   [MetadataIndexingOptions](https://apireference.groupdocs.com/net/search/groupdocs.search.options/textoptions/properties/metadataindexingoptions) is an object for specifying metadata indexing options.
*   [UseInlineStyles](https://apireference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/properties/useinlinestyles) is a flag to specify whether inline styles or CSS class are used to highlight occurrences.
*   [HighlightColor](https://apireference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/properties/highlightcolor) is a color used to highlight occurrences.
*   [TermHighlightStartTag](https://apireference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/properties/termhighlightstarttag) is the start tag of the highlighting of the found word. This tag is used only when highlighting in plain text.
*   [TermHighlightEndTag](https://apireference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/properties/termhighlightendtag) is the end tag of the highlighting of the found word. This tag is used only when highlighting in plain text.

The other options are used for highlighting occurrences in text fragments.

To highlight search results in the text of the whole document, a highlighter of the [DocumentHighlighter](https://apireference.groupdocs.com/net/search/groupdocs.search.highlighters/documenthighlighter) class is used. To create a highlighter of this type, you must pass an object of a class derived from the abstract class [OutputAdapter](https://apireference.groupdocs.com/net/search/groupdocs.search.common/outputadapter) to its constructor. Details on the output adapters are presented on the page [Output adapters]({{< ref "search/net/developer-guide/advanced-usage/searching/output-adapters.md" >}}).

If after generation the text of a document was saved to a file, this file can be opened by an Internet browser to navigate the occurrences of the words found. To navigate the occurrences, the following text is added to the URL in a browser:

#hitN

where N is a number starting from zero. The full document URL in a browser might look like this:

[file:///C:/Text.html#hit0](file:///C:/Text.html#hit0)

The following example demonstrates how to highlight search results in the text of an entire document.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";

// Creating an index settings instance
IndexSettings settings = new IndexSettings();
settings.TextStorageSettings = new TextStorageSettings(Compression.High); // Enabling storage of extracted text in the index

// Creating an index in the specified folder
Index index = new Index(indexFolder, settings);

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

## Hit highlighting in text fragments

Occurrences can also be highlighted in separate HTML or plain text fragments. For this, the highlighter of the [FragmentHighlighter](https://apireference.groupdocs.com/net/search/groupdocs.search.highlighters/fragmenthighlighter) class is used. The following properties are presented in the [HighlightOptions](https://apireference.groupdocs.com/net/search/groupdocs.search.options/highlightoptions) class to use with the highlighter of this type:

*   [TermsBefore](https://apireference.groupdocs.com/net/search/groupdocs.search.options/highlightoptions/properties/termsbefore) is used to specify the maximum number of words in a text fragment before highlighted word. The value must be in the range from 0 to 10000. The default value is 7.
*   [TermsAfter](https://apireference.groupdocs.com/net/search/groupdocs.search.options/highlightoptions/properties/termsafter) is used to specify the maximum number of words in a text fragment after highlighted word. The value must be in the range from 0 to 10000. The default value is 7.
*   [TermsTotal](https://apireference.groupdocs.com/net/search/groupdocs.search.options/highlightoptions/properties/termstotal) is used to specify the maximum number of words in a text fragment. The value must be in the range from 0 to 10000. The default value is 21.
*   [TermHighlightStartTag](https://apireference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/properties/termhighlightstarttag) is the start tag of the highlighting of the found word. This tag is used only when highlighting in plain text.
*   [TermHighlightEndTag](https://apireference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/properties/termhighlightendtag) is the end tag of the highlighting of the found word. This tag is used only when highlighting in plain text.

Generated text fragments with highlighted occurrences can be used, for example, to generate web pages containing search results on a site.

The example below demonstrates how to highlight search results in separate text fragments.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index settings instance
IndexSettings settings = new IndexSettings();
settings.TextStorageSettings = new TextStorageSettings(Compression.High); // Enabling storage of extracted text in the index

// Creating an index in the specified folder
Index index = new Index(indexFolder, settings);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Search for the word 'Einstein'
SearchResult result = index.Search("Einstein");

// Assigning highlight options
HighlightOptions options = new HighlightOptions();
options.TermsBefore = 5;
options.TermsAfter = 5;
options.TermsTotal = 15;
options.HighlightColor = new Color(0, 0, 127);
options.UseInlineStyles = true;

// Highlighting found words in separate text fragments of a document
FoundDocument document = result.GetFoundDocument(0);
FragmentHighlighter highlighter = new FragmentHighlighter(OutputFormat.Html);
index.Highlight(document, highlighter, options);

// Getting the result
FragmentContainer[] fragmentContainers = highlighter.GetResult();
for (int i = 0; i < fragmentContainers.Length; i++)
{
    FragmentContainer container = fragmentContainers[i];
    string[] fragments = container.GetFragments();
    if (fragments.Length > 0)
    {
        Console.WriteLine(container.FieldName);
        Console.WriteLine();
        for (int j = 0; j < fragments.Length; j++)
        {
            // Printing HTML markup to console
            Console.WriteLine(fragments[j]);
            Console.WriteLine();
        }
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
