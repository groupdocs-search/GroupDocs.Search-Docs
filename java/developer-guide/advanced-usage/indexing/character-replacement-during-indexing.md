---
id: character-replacement-during-indexing
url: search/java/character-replacement-during-indexing
title: Character replacement during indexing
weight: 1
description: "Character replacement during indexing can be used, for example, to convert all text to lowercase characters or to remove diacritics from text."
keywords: convert all text to lowercase, Character replacement during indexing, Character replacement
productName: GroupDocs.Search for Java
hideChildren: False
---
Character replacement during indexing can be used, for example, to convert all text to lowercase characters or to remove diacritics from text. Such replacements can reduce the size of an index on disk if the case of characters or diacritics are not significant. See also [Character replacements]({{< ref "search/java/developer-guide/advanced-usage/managing-dictionaries/character-replacements.md" >}}) page in the [Managing dictionaries]({{< ref "search/java/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The example below demonstrates how to configure and use character replacements during indexing.



```java
String indexFolder = "c:\\MyIndex\\";
String documentFolder = "c:\\MyDocuments\\";
 
// Enabling character replacements in the index settings
IndexSettings settings = new IndexSettings();
settings.setUseCharacterReplacements(true);
 
// Creating an index in the specified folder
Index index = new Index(indexFolder, settings);
 
// Configuring character replacements
// Deleting all existing character replacements from the dictionary
index.getDictionaries().getCharacterReplacements().clear();
// Creating new character replacements
CharacterReplacementPair[] characterReplacements = new CharacterReplacementPair[Character.MAX_VALUE + 1];
for (int i = 0; i < characterReplacements.length; i++)
{
    char character = (char)i;
    char replacement = Character.toLowerCase(character);
    characterReplacements[i] = new CharacterReplacementPair(character, replacement);
}
// Adding character replacements to the dictionary
index.getDictionaries().getCharacterReplacements().addRange(characterReplacements);
 
// Indexing documents from the specified folder
index.add(documentFolder);
 
// Searching in the index
// Case-sensitive search is no longer possible for this index, since all characters are lowercase
// By default, case-insensitive search is performed
SearchResult result = index.search("Einstein");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
