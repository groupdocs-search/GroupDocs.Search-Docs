---
id: fuzzy-search
url: search/java/fuzzy-search
title: Fuzzy search
weight: 6
description: "This article gives the knowledge about the fuzzy search definition, and its capabilities in GroupDocs.Search using Java search API."
keywords: fuzzy search
productName: GroupDocs.Search for Java
hideChildren: False
---
This page provides a fuzzy search definition, a description of its capabilities in GroupDocs.Search, and fuzzy search Java examples with use of GroupDocs.Search.

## Fuzzy search definition

Fuzzy search is a text search technique based on fuzzy logic that matches even where search queries or the text of documents to search for are written with errors.

## Fuzzy search features

Fuzzy search allows you to search for words that do not exactly match the search query. This type of search can be used, for example, to search for words containing spelling errors in documents. During fuzzy search, the number of possible differences with the search word is determined by the fuzzy search algorithm used. Differences or errors mean insertions, deletions, substitutions and, optionally, transpositions of adjacent characters.

GroupDocs.Search does not use special fuzzy queries. Instead, you can enable fuzzy search in search options for any search query, whether it is a single word, phrase or logical search query. And it becomes a fuzzy query.

The fuzzy search algorithm used is defined in the [SearchOptions](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/SearchOptions) class. By default, the algorithm provided by the [SimilarityLevel](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/SimilarityLevel) class is used with the similarity level value of 0.5. This algorithm uses a linear dependence of the number of possible differences from the search word. To calculate the maximum number of possible differences, the following expression is used:

int maxMistakeCount = (int)((1 - similarityLevel) \* termLength);

where similarityLevel is a parameter of the fuzzy search algorithm; termLength is the length of the searched word.

An example of setting a fuzzy search algorithm of this type is presented below.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
String query = "Einstein";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
SearchOptions options = new SearchOptions();
options.getFuzzySearch().setEnabled(true); // Enabling the fuzzy search
options.getFuzzySearch().setFuzzyAlgorithm(new SimilarityLevel(0.8)); // Creating the fuzzy search algorithm
// This function specifies 0 as the maximum number of mistakes for words from 1 to 4 characters.
// It specifies 1 as the maximum number of mistakes for words from 5 to 9 characters.
// It specifies 2 as the maximum number of mistakes for words from 10 to 14 characters. And so on.
 
// Search in index
SearchResult result = index.search(query, options);
```

The fuzzy search algorithm can also be specified by a table of correspondences between the length of the searched word and the maximum number of possible differences. For this, the algorithm presented by the [TableDiscreteFunction](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/TableDiscreteFunction) class is used. In this case, the correspondence table can be calculated based on the parameters of a step function. An example of setting a fuzzy search algorithm in the form of a step function is presented below.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
String query = "Einstein";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
SearchOptions options = new SearchOptions();
options.getFuzzySearch().setEnabled(true); // Enabling the fuzzy search
options.getFuzzySearch().setFuzzyAlgorithm(new TableDiscreteFunction(1, new Step(5, 2), new Step(8, 3))); // Creating the fuzzy search algorithm
// This function specifies 1 as the maximum number of mistakes for words from 1 to 4 characters.
// It specifies 2 as the maximum number of mistakes for words from 5 to 7 characters.
// It specifies 3 as the maximum number of mistakes for words from 8 and more characters.
 
// Search in index
SearchResult result = index.search(query, options);
```

The fuzzy search options object allows you to specify the following options:

*   The [setFuzzyAlgorithm](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/FuzzySearchOptions#setFuzzyAlgorithm(com.groupdocs.search.options.FuzzyAlgorithm)) method sets the fuzzy search algorithm. The currently available fuzzy search algorithms are [SimilarityLevel](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/SimilarityLevel) and [TableDiscreteFunction](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/TableDiscreteFunction). The default value is an instance of [SimilarityLevel](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/SimilarityLevel) class with a similarity level value of 0.5.
*   The [setOnlyBestResults](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/FuzzySearchOptions#setOnlyBestResults(boolean)) method sets a value indicating whether to return only the best results or not. The default value is false, which means that all results will be returned.
*   The [setOnlyBestResultsRange](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/FuzzySearchOptions#setOnlyBestResultsRange(byte)) method sets a value of exceeding the number of differences from the best result. The default value is 0.
*   The [setConsiderTranspositions](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/FuzzySearchOptions#setConsiderTranspositions(boolean)) method sets a value indicating whether to consider the transposition of adjacent characters as one mistake or not. The default value is true.
*   The [setEnabled](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/FuzzySearchOptions#setEnabled(boolean)) method enables or disables the fuzzy search. The default value is false.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
