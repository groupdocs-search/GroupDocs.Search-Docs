---
id: groupdocs-search-for-java-20-11-release-notes
url: search/java/groupdocs-search-for-java-20-11-release-notes
title: GroupDocs.Search for Java 20.11 Release Notes
weight: 10
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 20.11{{< /alert >}}

## Major Features

There are the following improvementes in this release:

  - Implement methods to get synonym groups
  - Implement methods to get homophone groups

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2435 | Implement methods to get synonym groups   | Improvement |
| SEARCHNET-2445 | Implement methods to get homophone groups | Improvement |

## Public API and Backward Incompatible Changes

### Implement methods to get synonym groups

This improvement adds a method to get synonyms for a specific word from the synonym dictionary. Also, this improvement adds methods for getting the groups of synonyms to which a certain word belongs in the dictionary, and for getting all groups of synonyms contained in the dictionary.

##### Public API changes

Method **String[] getSynonyms(String)** has been added to **com.groupdocs.search.dictionaries.SynonymDictionary** class.  
Method **String[]\[] getSynonymGroups(String)** has been added to **com.groupdocs.search.dictionaries.SynonymDictionary** class.  
Method **String[]\[] getAllSynonymGroups()** has been added to **com.groupdocs.search.dictionaries.SynonymDictionary** class.

##### Use cases

The following example demonstrates how to get synonyms for a word and how to get synonym groups to which a word belongs to.

**Java**

```java
// Creating an index in memory with default synonym dictionary
Index index = new Index();

// Getting synonyms for word 'make'
String[] synonyms = index.getDictionaries().getSynonymDictionary().getSynonyms("make");
System.out.println("Synonyms for 'make':");
for (String synonym : synonyms) {
    System.out.println(synonym);
}

// Getting groups of synonyms to which word 'make' belongs to
String[][] groups = index.getDictionaries().getSynonymDictionary().getSynonymGroups("make");
System.out.println("Synonym groups for 'make':");
for (String[] group : groups) {
    for (String group1 : group) {
        System.out.print(group1 + " ");
    }
    System.out.println();
}
```

### Implement methods to get homophone groups

This improvement adds a method to get homophones for a specific word from the homophone dictionary. Also, this improvement adds methods for getting the groups of homophones to which a certain word belongs in the dictionary, and for getting all groups of homophones contained in the dictionary.

##### Public API changes

Method **String[] getHomophones(String)** has been added to **com.groupdocs.search.dictionaries.HomophoneDictionary** class.  
Method **String[]\[] getHomophoneGroups(String)** has been added to **com.groupdocs.search.dictionaries.HomophoneDictionary** class.  
Method **String[]\[] getAllHomophoneGroups()** has been added to **com.groupdocs.search.dictionaries.HomophoneDictionary** class.

The following example demonstrates how to get homophones for a word and how to get homophone groups to which a word belongs to.

**Java**

```java
// Creating an index in memory with default homophone dictionary
Index index = new Index();

// Getting homophones for word 'braid'
String[] homophones = index.getDictionaries().getHomophoneDictionary().getHomophones("braid");
System.out.println("Homophones for 'braid':");
for (String homophone : homophones) {
    System.out.println(homophone);
}

// Getting groups of homophones to which word 'braid' belongs to
String[][] groups = index.getDictionaries().getHomophoneDictionary().getHomophoneGroups("braid");
System.out.println("Homophone groups for 'braid':");
for (String[] group : groups) {
    for (String group1 : group) {
        System.out.print(group1 + " ");
    }
    System.out.println();
}
```
