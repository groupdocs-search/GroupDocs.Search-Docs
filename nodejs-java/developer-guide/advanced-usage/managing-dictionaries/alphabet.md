---
id: alphabet
url: search/nodejs-java/alphabet
title: Alphabet
weight: 2
description: "This article gives the knowledge of the API methods which can be used to perform operations about Alphabets using Java."
keywords: Alphabets
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
An instance of the [Alphabet](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/Alphabet) class contains types of characters used for indexing. For detailed information on the types of characters, see the [Character types]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/character-types.md" >}}) page.

The [getCharacterType](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/Alphabet#getCharacterType(char)) method is used to get the type of a specific character.

The [getCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/Alphabet#getCount()) method returns the number of characters of a type other than [CharacterType.Separator](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/CharacterType#Separator).

To set the type of characters in the alphabet, use the [setRange](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/Alphabet#setRange(char%5B%5D,%20int)) method.

To set the [CharacterType.Separator](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/CharacterType#Separator) type for all characters in the alphabet, use the [clear](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/Alphabet#clear()) method.

To export types of all characters to a file, use the [exportDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#exportDictionary(java.lang.String)) method.

To import types of all characters from a file, use the [importDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#importDictionary(java.lang.String)) method.

The following example demonstrates the use of methods of the alphabet.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating or opening an index from the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Export the alphabet to a file
const fileName = Utils.OutputPath + 'AdvancedUsage/ManagingDictionaries/alphabet/Alphabet.dat';
index.getDictionaries().getAlphabet().exportDictionary(fileName);

if (index.getDictionaries().getAlphabet().getCount() > 0) {
  // Setting a type of all characters to Separator
  index.getDictionaries().getAlphabet().clear();
}

// Import the alphabet from a file
index.getDictionaries().getAlphabet().importDictionary(fileName);

if (
  String(index.getDictionaries().getAlphabet().getCharacterType(java.newChar('-'))) !=
  String(groupdocs.search.CharacterType.Blended)
) {
  // Setting a type of hyphen character to Blended
  index
    .getDictionaries()
    .getAlphabet()
    .setRange(java.newArray('char', ['-']), groupdocs.search.CharacterType.Blended);
}

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search in the index
const query = 'Elliot-Murray-Kynynmound';
const result = index.search(query);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
