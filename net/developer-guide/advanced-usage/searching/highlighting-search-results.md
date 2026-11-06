---
id: highlighting-search-results
url: search/net/highlighting-search-results
title: Highlighting search results
weight: 8
description: "This article describes how to highlight search results in the text of a document."
keywords: highlight search results
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page describes how to highlight search results in the text of a document.

## Hit highlighting in the text of entire document

After performing a search, occurrences of found words and phrases for a particular document can be highlighted in the text of the document using the [Highlight](https://reference.groupdocs.com/search/net/groupdocs.search/index/highlight/) method of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class. To do this, a highlighter of the corresponding type must be passed as an argument to the method.

The found document must come from a search of the same index or from a trusted source. Since version 26.11, a document that the index does not contain, for example a document deleted from the index after the search, is not highlighted: the highlighter gets no text, and the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index reports "Document is not found." ("Document item is not found." for an item of a container). Before, an index that does not store the texts of documents extracted the file at the path of any found document. An indexed document is highlighted by its path, so an application that gets found documents from its users, for example serialized with the [Serialize](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/serialize/) method, must check that a user may read the document.

Since version 26.11, a term is highlighted only in the fields of the document in which the search has found it: for example, a query of a field, such as "content: Einstein", highlights nothing in the other fields. Before, a term found in one field was highlighted in every field of the document.

The Index class also provides an overload of the [Highlight](https://reference.groupdocs.com/search/net/groupdocs.search/index/highlight/) method, which takes an object of the [HighlightOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/) class as an argument. The [HighlightOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/) class contains properties for setting the following options:

*   [CustomExtractor](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/customextractor/) is an extractor used during indexing, it is necessary if the text of the document was not saved in the index.
*   [AdditionalFields](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/additionalfields/) are additional document fields added during document indexing which are also necessary if the document text was not saved in the index.
*   [WordSplitter](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/wordsplitter/) is the custom word splitter used during indexing (since version 26.11); see [Highlighting documents indexed with a custom word splitter](#highlighting-documents-indexed-with-a-custom-word-splitter) below.
*   [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/cancellation/) is an object used to cancel the operation.
*   [GenerateHead](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/generatehead/) is a flag to specify whether the Head tag is generated in the output HTML. The default value is true.
*   [MetadataIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/metadataindexingoptions/) is an object for specifying metadata indexing options. This property and the other properties inherited from the [BaseOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/) class (for example, [Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/encoding/), [AutoDetectEncoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/autodetectencoding/) and [OcrIndexingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/ocrindexingoptions/)) specify how the text of the document is extracted; they are necessary if the document text was not saved in the index. Since version 26.11, an HTML document is read in the encoding of its indexing, which the index keeps when [IndexingOptions.Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/encoding/) or the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event sets it, as for a text document; the Encoding option is used only for an HTML document whose encoding the index does not keep, for example a document indexed by an earlier version.
*   [UseInlineStyles](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/useinlinestyles/) is a flag to specify whether inline styles or CSS class are used to highlight occurrences. The default value is true.
*   [HighlightColor](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/highlightcolor/) is a color used to highlight occurrences. The default value is (255, 216, 0).
*   [TermHighlightStartTag](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/termhighlightstarttag/) is the start tag of the highlighting of the found word. This tag is used only when highlighting in plain text.
*   [TermHighlightEndTag](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/termhighlightendtag/) is the end tag of the highlighting of the found word. This tag is used only when highlighting in plain text.

The other options are used for highlighting occurrences in text fragments.

To highlight search results in the text of the whole document, a highlighter of the [DocumentHighlighter](https://reference.groupdocs.com/search/net/groupdocs.search.highlighters/documenthighlighter/) class is used. To create a highlighter of this type, you must pass an object of a class derived from the abstract class [OutputAdapter](https://reference.groupdocs.com/search/net/groupdocs.search.common/outputadapter/) to its constructor. Details on the output adapters are presented on the page [Output adapters]({{< ref "search/net/developer-guide/advanced-usage/searching/output-adapters.md" >}}).

If after generation the text of a document was saved to a file, this file can be opened by an Internet browser to navigate the occurrences of the words found. To navigate the occurrences, the following text is added to the URL in a browser:

#hitN

where N is a number starting from zero. The full document URL in a browser might look like this:

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

Occurrences can also be highlighted in separate HTML or plain text fragments. For this, the highlighter of the [FragmentHighlighter](https://reference.groupdocs.com/search/net/groupdocs.search.highlighters/fragmenthighlighter/) class is used. The following properties are presented in the [HighlightOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/) class to use with the highlighter of this type:

*   [TermsBefore](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/termsbefore/) is used to specify the maximum number of words in a text fragment before highlighted word. The value must be in the range from 0 to 10000. The default value is 7.
*   [TermsAfter](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/termsafter/) is used to specify the maximum number of words in a text fragment after highlighted word. The value must be in the range from 0 to 10000. The default value is 7.
*   [TermsTotal](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/termstotal/) is used to specify the maximum number of words in a text fragment. The value must be in the range from 0 to 10000. The default value is 21.
*   [TermHighlightStartTag](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/termhighlightstarttag/) is the start tag of the highlighting of the found word. This tag is used only when highlighting in plain text.
*   [TermHighlightEndTag](https://reference.groupdocs.com/search/net/groupdocs.search.options/highlightoptions/termhighlightendtag/) is the end tag of the highlighting of the found word. This tag is used only when highlighting in plain text.

Generated text fragments with highlighted occurrences can be used, for example, to generate web pages containing search results on a site. Since version 26.11, the fragments of a field hold the words of that field only, and the separators at the end of a field, for example the final period, end its last fragment. Before, the first fragment of a field could begin with the last words of the previous field.

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

if (result.DocumentCount > 0)
{
    // Highlighting found words in separate text fragments of a document
    FoundDocument document = result.GetFoundDocument(0); // Getting the first found document
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
}
```

## Highlighting documents indexed with a custom word splitter

A document that was indexed with a custom word splitter (see the [Custom text segmenter]({{< ref "search/net/developer-guide/advanced-usage/indexing/custom-text-segmenter.md" >}}) page) has to be divided into words by the same splitter when it is highlighted. Since version 26.11, the splitter is passed in the [WordSplitter](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/wordsplitter/) property of the highlight options. The text is divided into the words of the splitter, whether it is read from the index or the document is extracted again, the found words and phrases are highlighted, and the text between the words is kept. A word that the text does not have after the words before it, for example a word that the splitter has changed, is not highlighted. Without the splitter, the words that the built-in division does not give, for example the words of a text without spaces, are not highlighted. A word splitter cannot be sent to the shards of a search network: the [Highlight](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searcher/highlight/) method of the network throws ArgumentException for the options with a word splitter, as for the options with a custom extractor (see the [Highlighting results in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/highlighting-results-in-network.md" >}}) page).

Since version 26.11, an index that stores the texts of documents (see [Storing text of indexed documents]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}})) stores the whole text of a document indexed with a custom word splitter, so the highlighting gives the whole text of such a document. The stored text of a document indexed so by an earlier version has only the words of the splitter, without the text between them, until the document is indexed again.

The following example demonstrates the highlighting of documents indexed with a custom word splitter.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// The custom word splitter: MySplitter is a class of the application that implements the IWordSplitter interface
IWordSplitter splitter = new MySplitter();

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Using the custom word splitter to divide the text of documents into words
index.Events.FileIndexing += (sender, args) =>
{
    args.WordSplitter = splitter;
};

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Search for the word 'Einstein'
SearchResult result = index.Search("Einstein");

// Highlighting occurrences in the text
if (result.DocumentCount > 0)
{
    FoundDocument document = result.GetFoundDocument(0); // Getting the first found document
    OutputAdapter outputAdapter = new FileOutputAdapter(OutputFormat.Html, @"c:\Highlighted.html"); // Creating the output adapter
    Highlighter highlighter = new DocumentHighlighter(outputAdapter); // Creating the highlighter instance
    HighlightOptions options = new HighlightOptions(); // Creating the highlight options
    options.WordSplitter = splitter; // Dividing the text into words by the same splitter
    index.Highlight(document, highlighter, options); // Generating HTML formatted text with highlighted occurrences
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
