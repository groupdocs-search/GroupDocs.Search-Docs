---
id: groupdocs-search-for-java-20-6-release-notes
url: search/java/groupdocs-search-for-java-20-6-release-notes
title: GroupDocs.Search for Java 20.6 Release Notes
weight: 30
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 20.6{{< /alert >}}

## Major Features

There are the following new features and improvements in this release:

*   Improve formatting of text extracted from index

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2278 | Improve formatting of text extracted from index | Improvement |
| SEARCHJAVA-132 | Move AttributeChangeBatch class to common package | Breaking Change |


## Public API and Backward Incompatible Changes

### Improve formatting of text extracted from index {#GroupDocs.SearchforJava20.6-Improveformattingoftextextractedfromindex}

This improvement allows you to choose between two alternatives:

1.  Indexing is faster, but with loss of formatting quality in some
    cases (mostly relevant for PDF format).
2.  Improve the formatting of text extracted during indexing, but with
    loss of indexing speed (mainly relevant for the PDF format).

By default, the raw mode is used if possible.

##### Public API changes {#GroupDocs.SearchforJava20.6-PublicAPIchanges}

Method **boolean getUseRawTextExtraction()** has been added to **com.groupdocs.search.IndexSettings** class.  
Method **void setUseRawTextExtraction(boolean)** has been added to **com.groupdocs.search.IndexSettings** class.

##### Use cases {#GroupDocs.SearchforJava20.6-Usecases}

The following example demonstrates how to disable raw text extraction mode to improve formatting of extracted text:


```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Setting not to use of raw text extraction mode
IndexSettings settings = new IndexSettings();
settings.setUseRawTextExtraction(false);

// Creating an index
Index index = new Index(indexFolder, settings);

// Indexing documents in the document folder
index.add(documentFolder);
```

### Move AttributeChangeBatch class to common package {#GroupDocs.SearchforJava20.6-MoveAttributeChangeBatchclasstocommonpackage}

This improvement brings order to the grouping of classes into packages.

##### Public API changes {#GroupDocs.SearchforJava20.6-PublicAPIchanges.1}

Class **AttributeChangeBatch** has been moved from **com.groupdocs.search** to **com.groupdocs.search.common** package.  
Constructor **AttributeChangeBatch()** has been deleted from **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **AttributeChangeBatch create()** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.

##### Use cases {#GroupDocs.SearchforJava20.6-Usecases.1}

This example demonstrates how to add and remove attributes from indexed documents:

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Indexing documents in a document folder
index.add(documentFolder);
 
// Creating an attribute change container object
AttributeChangeBatch batch = AttributeChangeBatch.create();
// Adding one attribute to all indexed documents
batch.addToAll("public");
// Removing one attribute from one indexed document
batch.remove("c:\\MyDocuments\\KeyPoints.doc", "public");
// Adding two attributes to one indexed document
batch.add("c:\\MyDocuments\\KeyPoints.doc", "main", "key");
 
// Applying attribute changes in the index
index.changeAttributes(batch);
```

