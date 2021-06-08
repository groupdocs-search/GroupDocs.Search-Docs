---
id: groupdocs-search-for-java-19-12-release-notes
url: search/java/groupdocs-search-for-java-19-12-release-notes
title: GroupDocs.Search for Java 19.12 Release Notes
weight: 1
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
  
  

{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 19.12{{< /alert >}}

## Major Features

{{< alert style="danger" >}}In this version, we are introducing a new public API that has been designed to be intuitive and easy to use. For more information on the new API, please check the Public Docs section. The deprecated API was moved to the com.groupdocs.search.legacy package, so after upgrading to this version, you need to replace the use of the package across the entire project from com.groupdocs.search to com.groupdocs.search.legacy to resolve build issues.{{< /alert >}}

Other notable features and improvements:

*   Implement highlighting search results in short fragments
*   Enhance document metadata indexing with new formats
*   Implement indexing each letter as a separate word
*   Implement ability to remove paths from index

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-1967 | Implement highlighting search results in short fragments | Improvement |
| SEARCHNET-1970 | Enhance document metadata indexing with new formats | Improvement |
| SEARCHNET-2110 | Implement new public API | Improvement |
| SEARCHNET-2035 | Implement indexing each letter as a separate word | New Feature |
| SEARCHNET-2108 | Implement ability to remove paths from index | New Feature |

## Public API and Backward Incompatible Changes

{{< alert style="info" >}}This section lists public API changes that were introduced in GroupDocs.Search for Java 19.12 It includes not only new and obsoleted public methods, but also a description of any changes in the behavior behind the scenes in GroupDocs.Search which may affect existing code. Any behavior introduced that could be seen as a regression and modifies existing behavior is especially important and is documented here.{{< /alert >}}

### Implement highlighting search results in short fragments

This improvement allows highlighting the search results in separate short fragments of the text, and not in the whole document. A detailed description of the feature is presented in the documentation on the [Highlighting search results]({{< ref "search/java/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}) page.

##### Usecases

This example shows how to generate short HTML snippets with highlighted found terms:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating index
Index index = new Index(indexFolder);
 
// Adding documents to index
index.add(documentFolder);
 
// Searching
SearchResult result = index.search("hobbit");
 
// Highlighting found terms in short HTML snippets
if (result.getDocumentCount() > 0) {
    FoundDocument document = result.getFoundDocument(0);
    HtmlFragmentHighlighter highlighter = new HtmlFragmentHighlighter();
    index.highlight(document, highlighter);
 
    // Getting the result
    FragmentContainer[] fragmentContainers = highlighter.getResult();
    for (FragmentContainer container : fragmentContainers) {
        String[] fragments = container.getFragments();
        if (fragments.length > 0) {
            System.out.println(container.getFieldName());
            System.out.println();
            for (String fragment : fragments) {
                // Printing HTML markup to console
                System.out.println(fragment);
                System.out.println();
            }
        }
    }
}
```

### Enhance document metadata indexing with new formats

This improvement adds support for new document formats. These are mostly documents, the main content of which is not textual, therefore only the metadata of these documents is indexed:

*   MP3 – MPEG-2 Audio Layer III;
*   WAV – Waveform Audio File Format;
*   BMP – Bitmap Picture;
*   GIF – Graphical Interchange Format File;
*   JP2 – JPEG 2000 Core Image File;
*   PNG – Portable Network Graphics;
*   WEBP – WebP Image Format File;
*   TIFF – Tagged Image File Format;
*   EMF – Enhanced Windows Metafile;
*   WMF – Windows Metafile;
*   JPG – JPEG Image;
*   PSD – Adobe Photoshop Document;
*   DJVU – DjVu Image;
*   MPP – Microsoft Project File;
*   TORRENT – BitTorrent File;
*   VSD – Visio Drawing File;
*   VSS – Visio Stencil File;
*   DCM – DICOM Image;
*   AVI – Audio Video Interleave File;
*   MOV – Apple QuickTime Movie;
*   QT – Apple QuickTime Movie;
*   FLV – Animate Video File;
*   ASF – Advanced Systems Format File.

A complete list of supported formats is provided on the [Supported Document Formats]({{< ref "search/java/getting-started/supported-document-formats.md" >}}) page.

##### Usecases

None.

### Implement new public API

Implemented a new convenient intuitive public API. Full documentation for the new API is presented [here]({{< ref "search/java/_index.md" >}}).

All public types from the legacy **com.groupdocs.search** package have been moved to the **com.groupdocs.search.legacy** package and marked deprecated with the message: "This interface / class is deprecated and will be available until January 2020 (version 20.1)."

##### Usecases

None.

### Implement indexing each letter as a separate word

This feature is designed to work with hieroglyphic languages and allows you to index each character in the text as a separate word, regardless of the presence of separators.

##### Usecases

The example shows how to perform indexing and search for Chinese characters:



```java
tring indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating index
Index index = new Index(indexFolder);
 
// Setting SeparateWord character type for Chinese characters
StringBuilder stringBuilder = new StringBuilder();
for (char character = 0x4E00; character <= 0x9FFF; character++) { // Common
    stringBuilder.append(character);
}
for (char character = 0x3400; character <= 0x4DBF; character++) { // Rare
    stringBuilder.append(character);
}
char[] characters = new char[stringBuilder.length()];
stringBuilder.getChars(0, stringBuilder.length(), characters, 0);
index.getDictionaries().getAlphabet().setRange(characters, CharacterType.SeparateWord); // Setting character type
 
// Adding documents to index
index.add(documentFolder);
 
// Searching for the Unicode character U+4E50
SearchResult result = index.search("\u4E50");
```

### Implement ability to remove paths from index

This feature allows you to remove from an index paths added for indexing. When indexed paths are removed from an index, the index is updated and all removed documents and folders become inaccessible for search. Detailed information about this feature is presented on the [Delete indexed paths]({{< ref "search/java/developer-guide/advanced-usage/indexing/delete-indexed-paths.md" >}}) page.

##### Usecases

The example shows how to remove indexed paths from an index:



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder1 = "c:\\MyDocuments\\";
String documentsFolder2 = "c:\\MyDocuments2\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folders
index.add(documentsFolder1);
index.add(documentsFolder2);
 
// Getting indexed paths from the index
String[] indexedPaths1 = index.getIndexedPaths();
 
// Writing indexed paths to the console
System.out.println("Indexed paths:");
for (String path : indexedPaths1) {
    System.out.println("\t" + path);
}
 
// Deleting index path from the index
DeleteResult deleteResult = index.delete(new String[] { documentsFolder1 }, new UpdateOptions());
 
// Getting indexed paths after deletion
String[] indexedPaths2 = index.getIndexedPaths();
System.out.println("\nDeleted paths: " + deleteResult.getSuccessCount());
 
System.out.println("\nIndexed paths:");
for (String path : indexedPaths2) {
    System.out.println("\t" + path);
}
```
