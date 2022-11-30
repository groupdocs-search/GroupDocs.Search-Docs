---
id: groupdocs-search-for-java-22-11-release-notes
url: search/java/groupdocs-search-for-java-22-11-release-notes
title: com.groupdocs.search for Java 22.11 Release Notes
weight: 8
description: ""
keywords: 
productName: com.groupdocs.search for Java
hideChildren: False
---

{{< alert style="info" >}}This page contains release notes for com.groupdocs.search for Java 22.11{{< /alert >}}

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

This enhancement adds a new StructureOutputAdapter class that collects the extracted text separately for each document field. For more information on output adapters, see the [Output adapters](https://docs.groupdocs.com/search/java/output-adapters/) documentation page.

##### Public API changes

Class **StructureOutputAdapter** has been added to **com.groupdocs.search.common** package.  
Method **com.groupdocs.search.common.DocumentField[] getResult()** has been added to **com.groupdocs.search.common.StructureOutputAdapter** class.  
Constructor **StructureOutputAdapter(com.groupdocs.search.options.OutputFormat)** has been added to **com.groupdocs.search.common.StructureOutputAdapter** class.

##### Use cases

The following example demonstrates how to output the extractracted text with different output adapters.

```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";

// Creating an index settings instance
IndexSettings settings = new IndexSettings();
settings.setTextStorageSettings(new TextStorageSettings(Compression.High)); // Enabling the storage of extracted text in the index

// Creating an index in the specified folder
Index index = new Index(indexFolder, settings);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Getting list of indexed documents
DocumentInfo[] documents = index.getIndexedDocuments();

// Getting a document text
if (documents.length > 0) {
    DocumentInfo document = documents[0];

    // Output to a file
    FileOutputAdapter fileOutputAdapter = new FileOutputAdapter(OutputFormat.Html, "C:\\Text.html");
    index.getDocumentText(document, fileOutputAdapter);

    // Output to a stream
    try (OutputStream stream = new ByteArrayOutputStream()) {
        StreamOutputAdapter streamOutputAdapter = new StreamOutputAdapter(OutputFormat.Html, stream);
        index.getDocumentText(document, streamOutputAdapter);
    }

    // Output to a string
    StringOutputAdapter stringOutputAdapter = new StringOutputAdapter(OutputFormat.Html);
    index.getDocumentText(document, stringOutputAdapter);
    String htmlText = stringOutputAdapter.getResult();
    System.out.println(htmlText);

    // Output to a structure
    StructureOutputAdapter structureOutputAdapter = new StructureOutputAdapter(OutputFormat.PlainText);
    index.getDocumentText(document, structureOutputAdapter);
    DocumentField[] fields = structureOutputAdapter.getResult();
    System.out.println(document.toString());
    for (DocumentField field : fields) {
        System.out.println("\t" + field.getName());
    }
}
```

### Implement text extraction in plain text format

This enhancement implements the ability to output the text of the document in plain text format, both when obtaining the text of the document from the index and when highlighting the found words in the text. For more information, see the [Getting indexed documents](https://docs.groupdocs.com/search/java/getting-indexed-documents/) documentation page and the [Highlighting search results](https://docs.groupdocs.com/search/java/highlighting-search-results/) documentation page.

##### Public API changes

Enum **OutputFormat** has been added to **com.groupdocs.search.options** package.  
Field **com.groupdocs.search.options.OutputFormat Html** has been added to **com.groupdocs.search.options.OutputFormat** enum.  
Field **com.groupdocs.search.options.OutputFormat PlainText** has been added to **com.groupdocs.search.options.OutputFormat** enum.

Class **DocumentHighlighter** has been added to **com.groupdocs.search.highlighters** package.  
Constructor **DocumentHighlighter(com.groupdocs.search.common.OutputAdapter)** has been added to **com.groupdocs.search.highlighters.DocumentHighlighter** class.  
Method **com.groupdocs.search.common.OutputAdapter getOutputAdapter()** has been added to **com.groupdocs.search.highlighters.DocumentHighlighter** class.

Class **FragmentHighlighter** has been added to **com.groupdocs.search.highlighters** package.  
Constructor **FragmentHighlighter(com.groupdocs.search.options.OutputFormat)** has been added to **com.groupdocs.search.highlighters.FragmentHighlighter** class.  
Method **com.groupdocs.search.common.FragmentContainer[] getResult()** has been added to **com.groupdocs.search.highlighters.FragmentHighlighter** class.

Method **java.lang.String getTermHighlightEndTag()** has been added to **com.groupdocs.search.options.HighlightOptions** class.  
Method **void setTermHighlightEndTag(java.lang.String)** has been added to **com.groupdocs.search.options.HighlightOptions** class.  
Method **java.lang.String getTermHighlightStartTag()** has been added to **com.groupdocs.search.options.HighlightOptions** class.  
Method **void setTermHighlightStartTag(java.lang.String)** has been added to **com.groupdocs.search.options.HighlightOptions** class.

Method **com.groupdocs.search.options.OutputFormat getOutputFormat()** has been added to **com.groupdocs.search.common.ResultBuilderFactory** class.

Constructor **FileOutputAdapter(com.groupdocs.search.options.OutputFormat, java.lang.String)** has been added to **com.groupdocs.search.common.FileOutputAdapter** class.  
Constructor **StreamOutputAdapter(com.groupdocs.search.options.OutputFormat, java.io.OutputStream)** has been added to **com.groupdocs.search.common.StreamOutputAdapter** class.  
Constructor **StringOutputAdapter(com.groupdocs.search.options.OutputFormat)** has been added to **com.groupdocs.search.common.StringOutputAdapter** class.

##### Use cases

The following example demonstrates how to highlight search results in plain text format.

```java
String indexFolder = "c:\\MyIndex\\";
String documentFolder = "c:\\MyDocuments\\";

// Creating an index settings instance
IndexSettings settings = new IndexSettings();
settings.setTextStorageSettings(new TextStorageSettings(Compression.High)); // Enabling the storage of extracted text in the index

// Creating an index in the specified folder
Index index = new Index(indexFolder, settings);

// Indexing documents from the specified folder
index.add(documentFolder);

// Search for the word 'Universe'
SearchResult result = index.search("Universe");

// Highlighting occurrences in the text
if (result.getDocumentCount() > 0) {
    FoundDocument document = result.getFoundDocument(0); // Getting the first found document
    StructureOutputAdapter outputAdapter = new StructureOutputAdapter(OutputFormat.PlainText); // Creating the output adapter
    Highlighter highlighter = new DocumentHighlighter(outputAdapter); // Creating the highlighter instance
    HighlightOptions options = new HighlightOptions(); // Creating the highlight options
    options.setTermHighlightStartTag("<Term>"); // Setting the start tag for the found word
    options.setTermHighlightEndTag("</Term>"); // Setting the end tag for the found word
    index.highlight(document, highlighter, options); // Generating plain text with highlighted occurrences

    DocumentField[] fields = outputAdapter.getResult();
    System.out.println(document.toString());
    for (DocumentField field : fields) {
        // Printing field names of the found document
        System.out.println("\t" + field.getName());
    }
}
```
