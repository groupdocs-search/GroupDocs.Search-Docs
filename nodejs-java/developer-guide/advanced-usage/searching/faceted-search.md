---
id: faceted-search
url: search/nodejs-java/faceted-search
title: Faceted search
weight: 5
description: "This article gives the knowledge of the creation of faceted search queries using Java search API."
keywords: Faceted search, Faceted search definition
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page describes the creation of faceted search queries.

## Faceted search definition

Faceted search within GroupDocs.Search is a filtering of search results by setting valid document field names to search.

## Creating faceted search queries

Faceted search allows you to search only in certain fields of documents, for example, only in the content field or in the file name field. A simple faceted search example is presented below with queries in text and object form.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search in the content field with text query
const query1 = 'content: Pellentesque';
const result1 = index.search(query1);

// Search in the content field with object query
const wordQuery = groupdocs.search.SearchQuery.createWordQuery('Pellentesque');
const fieldQuery = groupdocs.search.SearchQuery.createFieldQuery(
  groupdocs.search.CommonFieldNames.Content,
  wordQuery,
);

const result2 = index.search(fieldQuery);
```

Faceted search can be combined with other types of searches using parentheses. The following faceted search example demonstrates the same multi-faceted search query in text and object form. Both queries search for documents in the name of which there are both the words "lorem" and "ipsum", or the documents in the contents of which contain the phrase "lectus eu aliquam" or the phrase "dignissim turpis".

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search with text query
const query1 = '(filename: (lorem AND ipsum)) OR (content: ("lectus eu aliquam" OR "dignissim turpis"))';
const result1 = index.search(query1);

// Search with object query
const word6Query = groupdocs.search.SearchQuery.createWordQuery('lorem');
const word7Query = groupdocs.search.SearchQuery.createWordQuery('ipsum');
const andQuery = groupdocs.search.SearchQuery.createAndQuery(word6Query, word7Query);
const filenameQuery = groupdocs.search.SearchQuery.createFieldQuery(
  groupdocs.search.CommonFieldNames.FileName,
  andQuery,
);

const word1Query = groupdocs.search.SearchQuery.createWordQuery('lectus');
const word2Query = groupdocs.search.SearchQuery.createWordQuery('eu');
const word3Query = groupdocs.search.SearchQuery.createWordQuery('aliquam');
const word4Query = groupdocs.search.SearchQuery.createWordQuery('dignissim');
const word5Query = groupdocs.search.SearchQuery.createWordQuery('turpis');

const phrase1Query = groupdocs.search.SearchQuery.createPhraseSearchQuery(word1Query, word2Query, word3Query);
const phrase2Query = groupdocs.search.SearchQuery.createPhraseSearchQuery(word4Query, word5Query);
const orQuery = groupdocs.search.SearchQuery.createOrQuery(phrase1Query, phrase2Query);
const contentQuery = groupdocs.search.SearchQuery.createFieldQuery(
  groupdocs.search.CommonFieldNames.Content,
  orQuery,
);

const rootQuery = groupdocs.search.SearchQuery.createOrQuery(filenameQuery, contentQuery);
const result2 = index.search(rootQuery);
```

## Using format specific fields

For each document format, there are standard fields that may be present in documents of this type. The library provides the following classes containing constants with the names of standard document fields: [EpubFieldNames](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/EpubFieldNames), [FictionBookFieldNames](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/FictionBookFieldNames), [MailFieldNames](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/MailFieldNames), [PresentationFieldNames](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/PresentationFieldNames), [SpreadsheetFieldNames](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SpreadsheetFieldNames), [WordsFieldNames](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/WordsFieldNames).

There are also fields that may be present in documents of any type. The names of such fields are represented in the [CommonFieldNames](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/CommonFieldNames) class.

An example of using standard field names of documents is presented in the following example.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search in the content field with text query
const query1 = groupdocs.search.WordsFieldNames.Company + ": Dycum";
const result1 = index.search(query1);

// Search in the content field with object query
const wordQuery = groupdocs.search.SearchQuery.createWordQuery("Dycum");
const fieldQuery = groupdocs.search.SearchQuery.createFieldQuery(groupdocs.search.WordsFieldNames.Company, wordQuery);
const result2 = index.search(fieldQuery);
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
