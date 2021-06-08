---
id: groupdocs-search-for-java-19-3-release-notes
url: search/java/groupdocs-search-for-java-19-3-release-notes
title: GroupDocs.Search for Java 19.3 Release Notes
weight: 4
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 19.3{{< /alert >}}

## Major Features

There are the following improvements in this release:

*   Implement event that notifies about search phase finished
*   Implement logging of indexing operations
*   Searching for a complete phrase with stop words
*   Implement Dictionary API enhamcements

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-659 | Implement event that notifies about search phase finished | Improvement |
| SEARCHNET-1833 | Implement logging of indexing operations | Improvement |
| SEARCHNET-1845 | Searching for a complete phrase with stop words | Improvement |
| SEARCHNET-1878 | Implement Dictionary API enhamcements | Improvement |

## Public API and Backward Incompatible Changes

{{< alert style="info" >}}This section lists public API changes that were introduced in GroupDocs.Search for Java 19.3. It includes not only new and obsoleted public methods, but also a description of any changes in the behavior behind the scenes in GroupDocs.Search which may affect existing code. Any behavior introduced that could be seen as a regression and modifies existing behavior is especially important and is documented here.{{< /alert >}}

### Implement event that notifies about search phase finished

This improvement adds event that notifies about search phase is finished and provides intermediate results.

##### Public API changes

Class **SearchPhase** has been added to **com.groupdocs.search** package.  
Static field **int AliasSubstitution** has been added to ********com.groupdocs.search****.**SearchPhase****** class.  
Static field **int KeyboardLayoutCorrection** has been added to **com.groupdocs.search******.**SearchPhase****** class.  
Static field **int ****SpellingCorrection** has been added to **com.groupdocs.search******.**SearchPhase****** class.  
Static field **int ****HomophoneSearch** has been added to **com.groupdocs.search******.**SearchPhase****** class.  
Static field **int ****SynonymSearch** has been added to **com.groupdocs.search******.**SearchPhase****** class.  
Static field **int ****WordFormsSearch** has been added to **com.groupdocs.search******.**SearchPhase****** class.  
Static field **int ****FuzzySearch** has been added to **com.groupdocs.search******.**SearchPhase****** class.  
Static field **int ****WildcardMatching** has been added to **com.groupdocs.search******.**SearchPhase****** class.  
Static field **int ****RegexMatching** has been added to **com.groupdocs.search******.**SearchPhase****** class.

Class **SearchPhaseEventArgs** has been added to **com.groupdocs.search** package.  
Method **int getSearchPhase()** has been added to **com.groupdocs.search******.**SearchPhaseEventArgs****** class.  
Method **String**** getQuery()** has been added to **com.groupdocs.search******.**SearchPhaseEventArgs****** class.  
Method **String\[\] getWords()** has been added to **com.groupdocs.search******.**SearchPhaseEventArgs****** class.

Field **Event<EventHandler<SearchPhaseEventArgs>> SearchPhaseCompleted** has been added to **com.groupdocs.search****.Index** class.

##### Usecases

This example shows how to use SearchPhaseCompleted event:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating index
Index index = new Index(indexFolder);

// Adding synonyms
index.getDictionaries().getSynonymDictionary().addRange(new String[][] { new String[] { "big", "large" } });
 
// Adding documents to index
index.addToIndex(documentFolder);

// Subscribing to the event
index.SearchPhaseCompleted.add(new EventHandler<SearchPhaseEventArgs>() {
    public void invoke(Object sender, SearchPhaseEventArgs args) {
        System.out.println("Phase " + args.getSearchPhase() + ": " + args.getWords().length);
    }
});

// Creating search parameters
SearchParameters parameters = new SearchParameters();
parameters.setUseCaseSensitiveSearch(false);
parameters.getKeyboardLayoutCorrector().setEnabled(true);
parameters.getSpellingCorrector().setEnabled(true);
parameters.getSpellingCorrector().setMaxMistakeCount(1);
parameters.setUseHomophoneSearch(true);
parameters.setUseSynonymSearch(true);
parameters.setUseWordFormsSearch(true);
parameters.getFuzzySearch().setEnabled(true);
parameters.getFuzzySearch().setFuzzyAlgorithm(new TableDiscreteFunction(1));

// Searching for word 'big'.
// Note that enabling many of search options at a time may give many results and take a long time.
SearchResults results = index.search("big", parameters);
```

### Implement logging of indexing operations

This improvement implements logging of main index operations to file '**log.txt**' inside index folder.

### Searching for a complete phrase with stop words

This improvement provides ability of searching phrases containing stop words. Stop words are words that are not included in an index to reduce index size.

##### Public API changes

None.

##### Usecases

This example shows how to search with stop words:

**C#**

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating index
Index index = new Index(indexFolder);

// Adding stop words to dictionary
// Note that words 'in' and 'these' are stop words by default. This line of code is here for demonstration purposes only.
index.getDictionaries().getStopWordDictionary().addRange(new String[] { "in", "these" });

// Adding documents to index
index.addToIndex(documentFolder);

// Searching for phrase without stop words
SearchResults results1 = index.search("\"information contained *1 *1 materials\"");

// Searching for phrase containing stop words
// This search gives the same results as the previous one
SearchResults results2 = index.search("\"information contained in these materials\"");
```

### Implement Dictionary API enhamcements

This improvement adds overloads of **addRange()** methods to dictionary classes.

##### Public API changes

Class **AliasReplacementPair** has been added to **GroupDocs.Search** namespace.  
Constructor **AliasReplacementPair(String, String)** has been added to ****GroupDocs.Search.**AliasReplacementPair****** class.  
Method **String getAlias()** has been added to ****GroupDocs.Search.******AliasReplacementPair********** class.  
Method **String getReplacement()** has been added to ****GroupDocs.Search.******AliasReplacementPair********** class.

Class **CharacterReplacementPair** has been added to **GroupDocs.Search** namespace.  
Constructor **CharacterReplacementPair(char, char)** has been added to **GroupDocs.Search.CharacterReplacementPair** class.  
Method **char getCharacter()** has been added to **GroupDocs.Search.CharacterReplacementPair** class.  
Method **char getReplacement()** has been added to **GroupDocs.Search.CharacterReplacementPair** class.

Method **addRange(Iterable<AliasReplacementPair>)** has been added to **GroupDocs.Search.AliasDictionary** class.  
Method **addRange(AliasReplacementPair\[\])** has been added to **GroupDocs.Search.AliasDictionary** class.

Method **addRange(Iterable<CharacterReplacementPair>)** has been added to **GroupDocs.Search.CharacterReplacementDictionary** class.  
Method **addRange(CharacterReplacementPair\[\])** has been added to **GroupDocs.Search.CharacterReplacementDictionary** class.

Method **addRange(String\[\])** has been added to **GroupDocs.Search.SpellingCorrector** class. 

Method **addRange(String\[\]\[\])** has been added to **GroupDocs.Search.HomophoneDictionary** class.

Method **addRange(String\[\])** has been added to **GroupDocs.Search.StopWordDictionary** class.

Method **removeRange(String\[\])** has been added to **GroupDocs.Search.StopWordDictionary** class.

Method **addRange(String\[\]\[\])** has been added to **GroupDocs.Search.SynonymDictionary** class.

##### Usecases

This example shows how to manage dictionaries:

**C#**

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
  
// Creating index
Index index = new Index(indexFolder);
  
// Adding alias for a query
index.getDictionaries().getAliasDictionary().addRange(new AliasReplacementPair[] { new AliasReplacementPair("query1", "\"Celestial mechanics\"") });
  
// Adding stop words
index.getDictionaries().getStopWordDictionary().addRange(new String[] { "i", "we", "you", "he", "she", "it" });
  
// Adding words to spelling corrector
index.getDictionaries().getSpellingCorrector().addRange(new String[] { "Newton", "Leibniz" });
  
// Indexing
index.addToIndex(documentFolder);
  
// Searching
SearchResults results = index.search("query1");
```
