---
id: groupdocs-search-for-java-21-3-release-notes
url: search/java/groupdocs-search-for-java-21-3-release-notes
title: GroupDocs.Search for Java 21.3 Release Notes
weight: 2
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 21.3{{< /alert >}}

## Major Features

There are the following improvementes in this release:

- Implement additional options for highlighting search results
- Implement serialization and deserialization of search results
- Replace public constants with enum values

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2029 | Implement additional options for highlighting search results | Improvement |
| SEARCHNET-2531 | Implement serialization and deserialization of search results | Improvement |
| SEARCHNET-2533 | Replace public constants with enum values | Improvement |

## Public API and Backward Incompatible Changes

### Implement additional options for highlighting search results

This improvement adds the following features:
- Setting the color for highlighting search results in the output HTML.
- Option to set the inline highlighting style or via CSS.
- Option not to generate Head section in the output HTML.

##### Public API changes

Class **Color** has been added to **com.groupdocs.search.options** package.  
Constructor **Color(int, int, int, int)** has been added to **com.groupdocs.search.options.Color** class.  
Constructor **Color(int, int, int)** has been added to **com.groupdocs.search.options.Color** class.  
Method **int getAlpha()** has been added to **com.groupdocs.search.options.Color** class.  
Method **int getRed()** has been added to **com.groupdocs.search.options.Color** class.  
Method **int getGreen()** has been added to **com.groupdocs.search.options.Color** class.  
Method **int getBlue()** has been added to **com.groupdocs.search.options.Color** class.

Method **boolean getUseInlineStyles()** has been added to **com.groupdocs.search.options.HighlightOptions** class.  
Method **void setUseInlineStyles(boolean)** has been added to **com.groupdocs.search.options.HighlightOptions** class.  
Method **Color getHighlightColor()** has been added to **com.groupdocs.search.options.HighlightOptions** class.  
Method **void setHighlightColor(Color value)** has been added to **com.groupdocs.search.options.HighlightOptions** class.

Method **boolean getGenerateHead()** has been added to **com.groupdocs.search.options.TextOptions** class.  
Method **void setGenerateHead(boolean value)** has been added to **com.groupdocs.search.options.TextOptions** class.

##### Use cases

The following example demonstrates the use of the new options.

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Indexing documents in a document folder
index.add(documentFolder);

// Searching in the index
SearchResult result = index.search("Einstein");
FoundDocument document = result.getFoundDocument(0);

// Highlighting search results in the text of a document
OutputAdapter outputAdapter = new FileOutputAdapter("c:\\Result.html");
Highlighter highlighter = new HtmlHighlighter(outputAdapter);
HighlightOptions options = new HighlightOptions();
options.setHighlightColor(new Color(0, 127, 0));
options.setGenerateHead(true);
options.setUseInlineStyles(false);
index.highlight(document, highlighter, options);
```

### Implement serialization and deserialization of search results

This improvement adds methods for serializing and deserializing search results to classes: **DocumentInfo**, **FoundDocument**, **FoundDocumentField**.
This can be useful, for example, to send serialized search results to a frontend and then use the serialized data as parameters for queries from the frontend.

##### Public API changes

Method **byte[] serialize()** has been added to **com.groupdocs.search.results.DocumentInfo** class.  
Method **DocumentInfo deserialize(byte[])** has been added to **com.groupdocs.search.results.DocumentInfo** class.

Method **byte[] serialize()** has been added to **com.groupdocs.search.results.FoundDocument** class.  
Method **FoundDocument deserialize(byte[])** has been added to **com.groupdocs.search.results.FoundDocument** class.

Method **byte[] serialize()** has been added to **com.groupdocs.search.results.FoundDocumentField** class.  
Method **FoundDocumentField deserialize(byte[])** has been added to **com.groupdocs.search.results.FoundDocumentField** class.

##### Use cases

The following example demonstrates how to serialize and deserialize search result objects.

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Indexing documents in a document folder
index.add(documentFolder);

// Searching in the index
SearchResult result = index.search("Einstein");
FoundDocument document = result.getFoundDocument(0);

// Serializing a found document object
byte[] bytes = document.serialize();

// Deserializing the found document object
FoundDocument restoredDocument = FoundDocument.deserialize(bytes);

// Using restored document for highlighting search results
OutputAdapter outputAdapter = new FileOutputAdapter("c:\\Result.html");
Highlighter highlighter = new HtmlHighlighter(outputAdapter);
index.highlight(restoredDocument, highlighter);
```

### Replace public constants with enum values

This improvement replaces classes containing integer constants with enumerations.

##### Public API changes

Enumeration **VersionUpdateResult** has been added to **com.groupdocs.search.common** package.  
Enumeration **DocumentStatus** has been added to **com.groupdocs.search.common** package.  
Enumeration **SearchPhase** has been added to **com.groupdocs.search.events** package.  
Enumeration **OperationType** has been added to **com.groupdocs.search.events** package.  
Enumeration **IndexStatus** has been added to **com.groupdocs.search.common** package.  
Enumeration **NumberOfThreads** has been added to **com.groupdocs.search.options** package.  
Enumeration **IndexType** has been added to **com.groupdocs.search.options** package.  
Enumeration **Compression** has been added to **com.groupdocs.search.options** package.  
Enumeration **DocumentSourceKind** has been added to **com.groupdocs.search.common** package.  
Enumeration **CharacterType** has been added to **com.groupdocs.search.dictionaries** package.

##### Use cases

None.
