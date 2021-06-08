---
id: phrase-search
url: search/java/phrase-search
title: Phrase search
weight: 14
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
This page contains a phrase search definition as well as a phrase search description with use of GroupDocs.Search API.

## Phrase search definition

Phrase search is a type of search that allows users to search for documents containing an exact sentence or phrase rather than containing a set of keywords in random order.

## Phrase search in GroupDocs.Search

Phrase search allows you to perform search for exact phrase and find a given sequence of words in the text of indexed documents. In text form, the following syntax is used to specify a phrase search query:

*   "word1 word2 word3 ..."

Quotation marks are required, for example:

string query = "\\"theory of relativity\\"";

Please note that if stop words are used in a search query, the phrase will still be found, however, instead of stop words, the search engine will try to substitute any words.

The following example demonstrates performing the phrase search with a query in text and object form.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search for the phrase 'theory of relativity' in text form
String query1 = "\"theory of relativity\"";
SearchResult result1 = index.search(query1);
 
// Search for the phrase 'theory of relativity' in object form
SearchQuery word1 = SearchQuery.createWordQuery("theory");
SearchQuery word2 = SearchQuery.createWordQuery("of");
SearchQuery word3 = SearchQuery.createWordQuery("relativity");
SearchQuery query2 = SearchQuery.createPhraseSearchQuery(word1, word2, word3);
SearchResult result2 = index.search(query2);
```

## Phrase search with wildcards

Phrase search other than words can also use two kinds of wildcard characters:

*   \* byte-number
*   \* byte-number ~~ byte-number

where byte-number is a number from 0 to 255.

The first pattern represents the exact number of unknown words in a phrase, for example \*2. The second pattern represents the range of the number of unknown words in a phrase, for example \*1~~2.

Phrase search query with wildcards is flexible enough to be used instead of span term query (span query).

The following example demonstrates the use of wildcards in phrase search queries in text and object form.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search for the phrase in text form
String query1 = "\"theory *1~~2 relativity\"";
SearchResult result1 = index.search(query1);
 
// Search for the phrase in object form
SearchQuery word1 = SearchQuery.createWordQuery("theory");
SearchQuery wildcard2 = SearchQuery.createWildcardQuery(1, 2);
SearchQuery word3 = SearchQuery.createWordQuery("relativity");
SearchQuery query2 = SearchQuery.createPhraseSearchQuery(word1, wildcard2, word3);
SearchResult result2 = index.search(query2);
 
// The search can find the following phrases:
// "theory of relativity"
// "theory of special relativity"
```

Phrase search can be combined with other types of searches. The specification of search queries in text form is presented on the [Query language specification]({{< ref "search/java/developer-guide/advanced-usage/searching/query-language-specification.md" >}}) page. A table of the possibility of nesting search queries in object form is presented on the [Nesting search queries in object form]({{< ref "search/java/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form.md" >}}) page.

The following example demonstrates the use of both wildcards representing words and characters in words.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search for the phrase in text form
String query1 = "\"Test?(0~1) *1~~2 relativity\"";
SearchResult result1 = index.search(query1);
 
// Search for the phrase in object form
WordPattern pattern = new WordPattern();
pattern.appendString("Test");
pattern.appendWildcard(0, 1);
SearchQuery wordPattern1 = SearchQuery.createWordPatternQuery(pattern);
SearchQuery wildcard2 = SearchQuery.createWildcardQuery(1, 2);
SearchQuery word3 = SearchQuery.createWordQuery("relativity");
SearchQuery query2 = SearchQuery.createPhraseSearchQuery(wordPattern1, wildcard2, word3);
SearchResult result2 = index.search(query2);
 
// The search can find the following phrases:
// "Test of special relativity"
// "Tests of special relativity"
// "Test of general relativity"
// "Tests of general relativity"
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
