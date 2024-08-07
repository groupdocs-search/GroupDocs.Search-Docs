---
id: highlighting-search-results
url: search/nodejs-java/highlighting-search-results
title: Highlighting search results
weight: 8
description: "This article gives knowledge on how to highlight search results in the text of a document."
keywords: highlight search results
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page describes how to highlight search results in the text of a document.

## Hit highlighting in the text of entire document

After performing a search, occurrences of found words and phrases for a particular document can be highlighted in the text of the document using the [highlight](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#highlight(com.groupdocs.search.results.FoundDocument,%20com.groupdocs.search.highlighters.Highlighter)) method of the [Index](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index) class. To do this, a highlighter of the corresponding type must be passed as an argument to the method.

The Index class also represents an overload of the [highlight](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#highlight(com.groupdocs.search.results.FoundDocument,%20com.groupdocs.search.highlighters.Highlighter)) method, which takes an object of the [HighlightOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions) class as an argument. The [HighlightOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions) class contains properties for setting the following options:

*   [setCustomExtractor](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions#setCustomExtractor(com.groupdocs.search.common.IFieldExtractor)) method sets an extractor used during indexing, it is necessary if the text of the document was not saved in the index.
*   [setAdditionalFields](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions#setAdditionalFields(com.groupdocs.search.common.DocumentField%5B%5D)) method sets additional document fields added during document indexing which are also necessary if the document text was not saved in the index.
*   [setCancellation](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions#setCancellation(com.groupdocs.search.common.Cancellation)) method sets an object used to cancel the operation.
*   [setGenerateHead](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions#setGenerateHead(boolean)) method sets a flag to specify whether the Head tag is generated in the output HTML.
*   [getMetadataIndexingOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/TextOptions#getMetadataIndexingOptions()) method returns an object for specifying metadata indexing options.
*   [setUseInlineStyles](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions#setUseInlineStyles(boolean)) method sets a flag to specify whether inline styles or CSS class are used to highlight occurrences.
*   [setHighlightColor](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions#setHighlightColor(com.groupdocs.search.options.Color)) method sets a color used to highlight occurrences.
*   [TermHighlightStartTag](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions#setTermHighlightStartTag(java.lang.String)) is the start tag of the highlighting of the found word. This tag is used only when highlighting in plain text.
*   [TermHighlightEndTag](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions#setTermHighlightEndTag(java.lang.String)) is the end tag of the highlighting of the found word. This tag is used only when highlighting in plain text.

The other options are used for highlighting occurrences in text fragments.

To highlight search results in the text of the whole document, a highlighter of the [DocumentHighlighter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.highlighters/DocumentHighlighter) class is used. To create a highlighter of this type, you must pass an object of a class derived from the abstract class [OutputAdapter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/OutputAdapter) to its constructor. Details on the output adapters are presented on the page [Output adapters]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/output-adapters.md" >}}).

If after generation the text of a document was saved to a file, this file can be opened by an Internet browser to navigate the occurrences of the words found. To navigate the occurrences, the following text is added to the URL in a browser:

#hitN

where N is a number starting from zero. The full document URL in a browser might look like this:

file:///C:/Text.html#hit0

The following example demonstrates how to highlight search results in the text of an entire document.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentFolder = 'c:/MyDocuments/';

// Creating an index settings instance
const settings = new groupdocs.search.IndexSettings();
settings.setTextStorageSettings(new groupdocs.search.TextStorageSettings(groupdocs.search.Compression.High)); // Enabling the storage of extracted text in the index

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder, settings);

// Indexing documents from the specified folder
index.add(documentFolder);

// Search for the word 'ipsum'
const result = index.search('ipsum');

// Highlighting occurrences in the text
if (result.getDocumentCount() > 0) {
  {
    const document = result.getFoundDocument(0); // Getting the first found document
    const outputAdapter = new groupdocs.search.FileOutputAdapter(groupdocs.search.OutputFormat.Html, 'c:/Result.html/'); // Creating an output adapter to a file
    const highlighter = new groupdocs.search.DocumentHighlighter(outputAdapter); // Creating the highlighter object
    const options = new groupdocs.search.HighlightOptions(); // Creating the highlight options
    options.setHighlightColor(new groupdocs.search.Color(150, 255, 150)); // Setting highlight color
    options.setUseInlineStyles(false); // Using CSS styles to highlight occurrences
    options.setGenerateHead(true); // Generating Head tag in output HTML
    index.highlight(document, highlighter, options); // Generating HTML formatted text with highlighted occurrences
  }
  {
    const document = result.getFoundDocument(0); // Getting the first found document
    const outputAdapter = new groupdocs.search.StructureOutputAdapter(groupdocs.search.OutputFormat.PlainText); // Creating the output adapter
    const highlighter = new groupdocs.search.DocumentHighlighter(outputAdapter); // Creating the highlighter instance
    const options = new groupdocs.search.HighlightOptions(); // Creating the highlight options
    options.setTermHighlightStartTag('<Term>'); // Setting the start tag for the found word
    options.setTermHighlightEndTag('</Term>'); // Setting the end tag for the found word
    index.highlight(document, highlighter, options); // Generating plain text with highlighted occurrences

    const fields = outputAdapter.getResult();
    console.log(document.toString());
    for (const field of fields) {
      // Printing field names of the found document
      console.log('\t' + field.getName());
    }
  }
}
```

## Hit highlighting in text fragments

Occurrences can also be highlighted in separate HTML or plain text fragments. For this, the highlighter of the [FragmentHighlighter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.highlighters/FragmentHighlighter) class is used. The following parameters are presented in the [HighlightOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions) class to use with the highlighter of this type:

*   [setTermsBefore](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions#setTermsBefore(int)) method is used to specify the maximum number of words in a text fragment before highlighted word. The value must be in the range from 0 to 10000. The default value is 7.
*   [setTermsAfter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions#setTermsAfter(int)) method is used to specify the maximum number of words in a text fragment after highlighted word. The value must be in the range from 0 to 10000. The default value is 7.
*   [setTermsTotal](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions#setTermsTotal(int)) is used to specify the maximum number of words in a text fragment. The value must be in the range from 0 to 10000. The default value is 21.
*   [TermHighlightStartTag](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions#setTermHighlightStartTag(java.lang.String)) is the start tag of the highlighting of the found word. This tag is used only when highlighting in plain text.
*   [TermHighlightEndTag](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/HighlightOptions#setTermHighlightEndTag(java.lang.String)) is the end tag of the highlighting of the found word. This tag is used only when highlighting in plain text.

Generated text fragments with highlighted occurrences can be used, for example, to generate web pages containing search results on a site.

The example below demonstrates how to highlight search results in separate text fragments.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentFolder = 'c:/MyDocuments/';

// Creating an index settings instance
const settings = new groupdocs.search.IndexSettings();
settings.setTextStorageSettings(new groupdocs.search.TextStorageSettings(groupdocs.search.Compression.High)); // Enabling the storage of extracted text in the index

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder, settings);

// Indexing documents from the specified folder
index.add(documentFolder);

// Search for the word 'ipsum'
const result = index.search('ipsum');

// Assigning highlight options
const options = new groupdocs.search.HighlightOptions();
options.setTermsBefore(5);
options.setTermsAfter(5);
options.setTermsTotal(15);
options.setHighlightColor(new groupdocs.search.Color(127, 200, 255));
options.setUseInlineStyles(true);

// Highlighting found words in separate text fragments of a document
const document = result.getFoundDocument(0);
const highlighter = new groupdocs.search.FragmentHighlighter(groupdocs.search.OutputFormat.Html);
index.highlight(document, highlighter, options);

// Getting the result
let text = '';
const fragmentContainers = highlighter.getResult();
for (const container of fragmentContainers) {
  const fragments = container.getFragments();
  if (fragments.length > 0) {
    text += '\n<br>';
    text += container.getFieldName();
    text += '<br>\n';
    for (const fragment of fragments) {
      // Printing HTML markup to console
      text += fragment;
      text += '\n';
    }
  }
}
console.log(text);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
