---
id: groupdocs-search-for-java-18-12-release-notes
url: search/java/groupdocs-search-for-java-18-12-release-notes
title: GroupDocs.Search for Java 18.12 Release Notes
weight: 1
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 18.12.{{< /alert >}}

## Major Features

There are 2 new features in this regular monthly release:

*   Implement blended characters
*   Implement wildcard search

## All Changes

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-803 | Implement blended characters | New Feature |
| SEARCHNET-1781 | Implement wildcard search | New Feature |

## Public API and Backward Incompatible Changes

{{< alert style="info" >}}This section lists public API changes that were introduced in GroupDocs.Search for Java 18.12. It includes not only new and obsoleted public methods, but also a description of any changes in the behavior behind the scenes in GroupDocs.Search which may affect existing code. Any behavior introduced that could be seen as a regression and modifies existing behavior is especially important and is documented here.{{< /alert >}}

### Implement blended characters

##### Description

This feature introduces a new class of characters - blended. When indexing, blended characters are interpreted simultaneously as valid letters and as separators.  
For example, if the hyphen is marked as a blended character then indexing of term 'silver-gray' will result in saving of 3 terms in the index: 'silver', 'gray', and 'silver-gray'.

##### Public API changes

Class **CharacterType** has been added to **com.groupdocs.search** package.  
Static field **byte Separator** has been added to **com.groupdocs.search****.****CharacterType** class.  
Static field **byte Letter** has been added to **com.groupdocs.search****.****CharacterType** class.  
Static field **byte Blended** has been added to **com.groupdocs.search****.****CharacterType** class.

Method **byte get\_Item(char)** has been added to **com.groupdocs.search****.Alphabet**class.  
Method **void setRange(char\[\] characters, byte)** nas been added to **com.groupdocs.search****.**Alphabet**** class.

##### Usecases

This example shows how to perform indexing and search with blended characters:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating index
Index index = new Index(indexFolder);
 
// Marking hyphen as blended character
index.getDictionaries().getAlphabet().setRange(new char[] { '-' }, CharacterType.Blended);
 
// Adding documents to index
index.addToIndex(documentFolder);
 
// Searching for word 'silver-gray'
SearchResults results = index.search("silver-gray");
```

### Implement wildcard search

##### Description

This feature allows performing search of words containing wildcards.  
There are two possible forms of a wildcard to use in wildcard search:

*   ? - quotation mark representing one arbitrary character;
*   ?(N~M) - the range of arbitrary characters in an amount from N to M, where N and M must be in the range from 0 to 255.

As well implemented the ability to perform wildcard search using the more flexible object form of the search query.

##### Public API changes

Class **WordPattern** has been added to **com.groupdocs.search** package.  
Constructor **WordPattern()** nas been added to **com.groupdocs.search******.**WordPattern****** class.  
Method **appendString(String)** nas been added to **com.groupdocs.search******.**WordPattern****** class.  
Method **appendCharacter(char)** nas been added to **com.groupdocs.search******.**WordPattern****** class.  
Method **appendOneCharacterWildcard()** nas been added to **com.groupdocs.search******.**WordPattern****** class.  
Method **appendZeroOrOneCharacterWildcard()** nas been added to **com.groupdocs.search******.**WordPattern****** class.  
Method **appendZeroOrMoreCharactersWildcard()** nas been added to **com.groupdocs.search******.**WordPattern****** class.  
Method **appendOneOrMoreCharactersWildcard()** nas been added to **com.groupdocs.search******.**WordPattern****** class.  
Method **appendWildcard(int, int)** nas been added to **com.groupdocs.search******.**WordPattern****** class.

Method ****SearchQuery c**reateWordPatternQuery(WordPattern)** nas been added to **com.groupdocs.search****.SearchQuery** class.

Method ****SearchQuery g**etChild(int)** nas been added to **com.groupdocs.search****.SearchQuery** class.

##### Usecases

The first example shows how to perform wildcard search using the query in text form:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating index
Index index = new Index(indexFolder);
 
// Adding documents to index
index.addToIndex(documentFolder);
 
// Searching for words 'affect' or 'effect' in a one document with 'principal', 'principle', 'principles', or 'principally'
SearchResults results1 = index.search("?ffect & princip?(2~4)");
 
// Searching with a single query for phrases 'assure equal opportunities', 'ensure equal opportunities', and 'sure equal opportunities'
SearchResults results2 = index.search("\"?(0~2)sure equal opportunities\"");
```

The next example shows how to perform wildcard search using a query in object form:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
  
// Creating index
Index index = new Index(indexFolder);
  
// Adding documents to index
index.addToIndex(documentFolder);
  
// Constructing query 1
// Word 1 in the query is a pattern '?ffect' for wildcard search
WordPattern pattert11 = new WordPattern();
pattert11.appendOneCharacterWildcard();
pattert11.appendString("ffect");
SearchQuery subquery11 = SearchQuery.createWordPatternQuery(pattert11);
  
// Word 2 in the query is a pattern 'princip?(2~4)' for wildcard search
WordPattern pattert12 = new WordPattern();
pattert12.appendString("princip");
pattert12.appendWildcard(2, 4);
SearchQuery subquery12 = SearchQuery.createWordPatternQuery(pattert12);
  
// Creating boolean search query
SearchQuery query1 = SearchQuery.createAndQuery(subquery11, subquery12);
  
// Searching with query 1
SearchResults results1 = index.search(query1, new SearchParameters());
  
// Constructing query 2
// Word 1 in the phrase is a pattern '?(0~2)sure' for wildcard search
WordPattern pattert21 = new WordPattern();
pattert21.appendWildcard(0, 2);
pattert21.appendString("sure");
SearchQuery subquery21 = SearchQuery.createWordPatternQuery(pattert21);
  
// Word 2 in the phrase is searched with different word forms ('equal', 'equals', 'equally', etc.)
SearchQuery subquery22 = SearchQuery.createWordQuery("equal");
subquery22.setSearchParameters(new SearchParameters());
subquery22.getSearchParameters().setUseWordFormsSearch(true);
  
// Word 3 in the phrase is searched with maximum 2 differences of fuzzy search
SearchQuery subquery23 = SearchQuery.createWordQuery("opportunities");
subquery23.setSearchParameters(new SearchParameters());
subquery23.getSearchParameters().getFuzzySearch().setEnabled(true);
subquery23.getSearchParameters().getFuzzySearch().setFuzzyAlgorithm(new TableDiscreteFunction(2));
  
// Creating phrase search query
SearchQuery query2 = SearchQuery.createPhraseSearchQuery(subquery21, subquery22, subquery23);
  
// Searching with query 2
SearchResults results2 = index.search(query2, new SearchParameters());
```
