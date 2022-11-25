---
id: faceted-search
url: search/java/faceted-search
title: Faceted search
weight: 5
description: "This article gives the knowledge of the creation of faceted search queries using Java search API."
keywords: Faceted search, Faceted search definition
productName: GroupDocs.Search for Java
hideChildren: False
---
This page describes the creation of faceted search queries.

## Faceted search definition

Faceted search within GroupDocs.Search is a filtering of search results by setting valid document field names to search.

## Creating faceted search queries

Faceted search allows you to search only in certain fields of documents, for example, only in the content field or in the file name field. A simple faceted search example is presented below with queries in text and object form.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search in the content field with text query
SearchResult result1 = index.search("content: Einstein");
 
// Search in the content field with object query
SearchQuery wordQuery = SearchQuery.createWordQuery("Einstein");
SearchQuery fieldQuery = SearchQuery.createFieldQuery(CommonFieldNames.Content, wordQuery);
SearchResult result2 = index.search(fieldQuery);
```

Faceted search can be combined with other types of searches using parentheses. The following faceted search example demonstrates the same multi-faceted search query in text and object form. Both queries search for documents in the name of which there are both the words "Albert" and "Einstein", or the documents in the contents of which contain the phrase "theory of relativity" or the phrase "special relativity".



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search with text query
SearchResult result1 = index.search("(filename: (Albert AND Einstein)) OR (content: (\"theory of relativity\" OR \"special relativity\"))");
 
// Search with object query
SearchQuery albertQuery = SearchQuery.createWordQuery("Albert");
SearchQuery einsteinQuery = SearchQuery.createWordQuery("Einstein");
SearchQuery andQuery = SearchQuery.createAndQuery(albertQuery, einsteinQuery);
SearchQuery filenameQuery = SearchQuery.createFieldQuery(CommonFieldNames.FileName, andQuery);
 
SearchQuery theoryQuery = SearchQuery.createWordQuery("theory");
SearchQuery ofQuery = SearchQuery.createWordQuery("of");
SearchQuery relativity1Query = SearchQuery.createWordQuery("relativity");
SearchQuery specialQuery = SearchQuery.createWordQuery("special");
SearchQuery relativity2Query = SearchQuery.createWordQuery("relativity");
 
SearchQuery phrase1Query = SearchQuery.createPhraseSearchQuery(theoryQuery, ofQuery, relativity1Query);
SearchQuery phrase2Query = SearchQuery.createPhraseSearchQuery(specialQuery, relativity2Query);
SearchQuery orQuery = SearchQuery.createOrQuery(phrase1Query, phrase2Query);
SearchQuery contentQuery = SearchQuery.createFieldQuery(CommonFieldNames.Content, orQuery);
 
SearchQuery rootQuery = SearchQuery.createOrQuery(filenameQuery, contentQuery);
SearchResult result2 = index.search(rootQuery);
```

## Using format specific fields

For each document format, there are standard fields that may be present in documents of this type. The library provides the following classes containing constants with the names of standard document fields: [EpubFieldNames](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/EpubFieldNames), [FictionBookFieldNames](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/FictionBookFieldNames), [MailFieldNames](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/MailFieldNames), [PresentationFieldNames](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/PresentationFieldNames), [SpreadsheetFieldNames](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/SpreadsheetFieldNames), [WordsFieldNames](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/WordsFieldNames).

There are also fields that may be present in documents of any type. The names of such fields are represented in the [CommonFieldNames](https://reference.groupdocs.com/search/java/com.groupdocs.search.options/CommonFieldNames) class.

An example of using standard field names of documents is presented in the following example.



```java
String indexFolder = "c:\\MyIndex";
String documentsFolder = "c:\\MyDocuments";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search in the content field with text query
String query1 = WordsFieldNames.Company + ": Dycum";
SearchResult result1 = index.search(query1);
 
// Search in the content field with object query
SearchQuery wordQuery = SearchQuery.createWordQuery("Dycum");
SearchQuery fieldQuery = SearchQuery.createFieldQuery(WordsFieldNames.Company, wordQuery);
SearchResult result2 = index.search(fieldQuery);
```

The following are the names of standard fields included in the library grouped by the formats containing them.

### Common fields

*   Content
    
*   FileName
    
*   FormatFamily
    
*   CreationDate
    
*   ModificationDate
    

### Epub format fields

*   Title
    
*   Subject
    
*   Author
    
*   Description
    
*   Language
    
*   Copyrights
    
*   Publisher
    
*   PublishedDate
    

### Fiction Book format fields

*   Title
    
*   Subject
    
*   Keywords
    
*   Author
    
*   Description
    
*   Language
    
*   Publisher
    
*   PublishedDate
    

### Mail format fields

*   MailMessageBody
    
*   MailSenderName
    
*   MailDisplayTo
    
*   MailSubject
    

### Presentation format fields

*   Application
    
*   ApplicationVersion
    
*   Title
    
*   Subject
    
*   Comments
    
*   Keywords
    
*   ContentStatus
    
*   Category
    
*   Manager
    
*   Author
    
*   LastAuthor
    
*   Company
    
*   HyperlinkBase
    
*   CreatedTime
    
*   LastSavedTime
    
*   LastPrintedTime
    
*   RevisionNumber
    
*   TotalEditingTime
    

### Spreadsheet format fields

*   Application
    
*   ApplicationVersion
    
*   Title
    
*   Subject
    
*   Comments
    
*   Keywords
    
*   ContentStatus
    
*   Category
    
*   Manager
    
*   Author
    
*   LastAuthor
    
*   Company
    
*   HyperlinkBase
    
*   CreatedTime
    
*   LastSavedTime
    
*   LastPrintedTime
    

### Words format fields

*   Application
    
*   ApplicationVersion
    
*   Template
    
*   Title
    
*   Subject
    
*   Comments
    
*   Keywords
    
*   ContentStatus
    
*   Category
    
*   Manager
    
*   Author
    
*   LastAuthor
    
*   Company
    
*   HyperlinkBase
    
*   CreatedTime
    
*   LastSavedTime
    
*   LastPrintedTime
    
*   RevisionNumber
    
*   TotalEditingTime
    

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
