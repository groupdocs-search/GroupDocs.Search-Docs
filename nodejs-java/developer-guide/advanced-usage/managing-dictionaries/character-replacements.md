---
id: character-replacements
url: search/nodejs-java/character-replacements
title: Character replacements
weight: 3
description: "This article gives the knowledge of the API methods which can be used to perform operations about Character replacements using Java."
keywords: Character replacements
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
An instance of the [CharacterReplacementDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary) class contains all the character replacements defined in an index. For detailed information on character replacement, see the [Character replacement during Indexing]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/character-replacement-during-indexing.md" >}}) page.

The [getCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#getCount()) method allows you to get the number of character replacements defined in the dictionary.

To add character replacements to the dictionary, use the [addRange](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#addRange(com.groupdocs.search.dictionaries.CharacterReplacementPair%5B%5D)) method.

To remove character replacements from the dictionary, the [removeRange](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#removeRange(char%5B%5D)) method is used.

The [contains](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#contains(char)) method is used to determine if the dictionary contains a replacement for the specified character.

To get a replacement for the specified character, use the [getReplacement](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#getReplacement(char)) method.

To remove all replacements from the dictionary, use the [clear](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/CharacterReplacementDictionary#clear()) method.

To export all replacements to a file, use the [exportDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#exportDictionary(java.lang.String)) method.

To import character replacements from a file, use the [importDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#importDictionary(java.lang.String)) method.

The following example demonstrates the use of the character replacement dictionary methods.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Enabling character replacements in the index settings
const settings = new groupdocs.search.IndexSettings();
settings.setUseCharacterReplacements(true);

// Creating an index from in specified folder
const index = new groupdocs.search.Index(indexFolder, settings, true);

if (index.getDictionaries().getCharacterReplacements().getCount() > 0) {
  // Deleting all character replacements from the dictionary
  index.getDictionaries().getCharacterReplacements().clear();
}

// Adding character replacement
const range = java.newArray('com.groupdocs.search.dictionaries.CharacterReplacementPair', [
  new groupdocs.search.CharacterReplacementPair(java.newChar('-'), java.newChar('~')),
]);
index.getDictionaries().getCharacterReplacements().addRange(range);

if (index.getDictionaries().getCharacterReplacements().contains(java.newChar('-'))) {
  const replacement = index.getDictionaries().getCharacterReplacements().getReplacement(java.newChar('-'));
  console.log('The replacement for hyphen is ' + replacement);

  // Deleting the hyphen character replacement from the dictionary
  index
    .getDictionaries()
    .getCharacterReplacements()
    .removeRange(java.newArray('char', ['-']));
}

// Creating new character replacements
const count = 65536;
const array = new Array(count);
for (let i = 0; i < count; i++) {
  const text = String.fromCharCode(i);
  const character = java.newChar(text);
  const replacement = java.newChar(text.toLowerCase()[0]);
  array[i] = new groupdocs.search.CharacterReplacementPair(character, replacement);
}
const characterReplacements = java.newArray('com.groupdocs.search.dictionaries.CharacterReplacementPair', array);
// Adding character replacements to the dictionary
index.getDictionaries().getCharacterReplacements().addRange(characterReplacements);

// Export character replacements to a file
const fileName =
  Utils.OutputPath + 'AdvancedUsage/ManagingDictionaries/characterReplacements/CharacterReplacements.dat';
index.getDictionaries().getCharacterReplacements().exportDictionary(fileName);

// Import character replacements from a file
index.getDictionaries().getCharacterReplacements().importDictionary(fileName);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search in the index
const query = 'Elliot';
const options = new groupdocs.search.SearchOptions();
options.setUseCaseSensitiveSearch(true);
const result = index.search(query, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
