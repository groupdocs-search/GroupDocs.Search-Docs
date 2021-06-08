---
id: character-replacements
url: search/java/character-replacements
title: Character replacements
weight: 3
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
An instance of the [CharacterReplacementDictionary](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary) class contains all the character replacements defined in an index. For detailed information on character replacement, see the [Character replacement during Indexing]({{< ref "search/java/developer-guide/advanced-usage/indexing/character-replacement-during-indexing.md" >}}) page.

The [getCount](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#getCount()) method allows you to get the number of character replacements defined in the dictionary.

To add character replacements to the dictionary, use the [addRange](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#addRange(com.groupdocs.search.dictionaries.CharacterReplacementPair%5B%5D)) method.

To remove character replacements from the dictionary, the [removeRange](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#removeRange(char%5B%5D)) method is used.

The [contains](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#contains(char)) method is used to determine if the dictionary contains a replacement for the specified character.

To get a replacement for the specified character, use the [getReplacement](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#getReplacement(char)) method.

To remove all replacements from the dictionary, use the [clear](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#clear()) method.

To export all replacements to a file, use the [exportDictionary](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/DictionaryBase#exportDictionary(java.lang.String)) method.

To import character replacements from a file, use the [importDictionary](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/DictionaryBase#importDictionary(java.lang.String)) method.

The following example demonstrates the use of the character replacement dictionary methods.



```java
String indexFolder = "c:\\MyIndex\\";
 
// Creating an index from in specified folder
Index index = new Index(indexFolder);
 
if (index.getDictionaries().getCharacterReplacements().getCount() > 0) {
    // Deleting all character replacements from the dictionary
    index.getDictionaries().getCharacterReplacements().clear();
}
 
if (index.getDictionaries().getCharacterReplacements().contains('-')) {
    char replacement = index.getDictionaries().getCharacterReplacements().getReplacement('-');
    System.out.println("The replacement for hyphen is " + replacement);
 
    // Deleting the hyphen character replacement from the dictionary
    index.getDictionaries().getCharacterReplacements().removeRange(new char[] { '-' });
}
 
// Export character replacements to a file
index.getDictionaries().getCharacterReplacements().exportDictionary("C:\\CharacterReplacements.dat");
 
// Import character replacements from a file
index.getDictionaries().getCharacterReplacements().importDictionary("C:\\CharacterReplacements.dat");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
