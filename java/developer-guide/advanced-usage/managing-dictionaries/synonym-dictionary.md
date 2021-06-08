---
id: synonym-dictionary
url: search/java/synonym-dictionary
title: Synonym dictionary
weight: 8
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
The [SynonymDictionary](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SynonymDictionary) class is designed to store synonyms in an index. For information on searching using synonyms, see the [Synonym search]({{< ref "search/java/developer-guide/advanced-usage/searching/synonym-search.md" >}}) page.

To get the number of synonyms in the dictionary, use the [getCount](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SynonymDictionary#getCount()) method.

To add groups of synonyms to the dictionary, use the [addRange](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SynonymDictionary#addRange(java.lang.Iterable)) method.

The [clear](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SynonymDictionary#clear()) method is used to remove all synonyms from the dictionary.

The [getSynonyms](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SynonymDictionary#getSynonyms(java.lang.String)) method is used to get a list of synonyms for a given word.

The [getSynonymGroups](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SynonymDictionary#getSynonymGroups(java.lang.String)) method is used to get all synonym groups to which a given word belongs.

To get all synonym groups from the dictionary, use the [getAllSynonymGroups](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SynonymDictionary#getAllSynonymGroups()) method.

To export synonyms to a file, use the [exportDictionary](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/DictionaryBase#exportDictionary(java.lang.String)) method.

To import synonyms from a file, use the [importDictionary](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/DictionaryBase#importDictionary(java.lang.String)) method.

The following example demonstrates the use of methods of the synonym dictionary.



```java
String indexFolder = "c:\\MyIndex\\";

// Creating an index from in specified folder
Index index = new Index(indexFolder);

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

if (index.getDictionaries().getSynonymDictionary().getCount() > 0) {
    // Removing all synonyms from the dictionary
    index.getDictionaries().getSynonymDictionary().clear();
}

// Adding synonyms to the dictionary
String[][] synonymGroups = new String[][] {
    new String[] { "achieve", "accomplish", "attain", "reach" },
    new String[] { "accept", "take", "have" },
};
index.getDictionaries().getSynonymDictionary().addRange(synonymGroups);

// Export synonyms to a file
index.getDictionaries().getSynonymDictionary().exportDictionary("C:\\Synonyms.dat");

// Import synonyms from a file
index.getDictionaries().getSynonymDictionary().importDictionary("C:\\Synonyms.dat");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
