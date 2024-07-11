---
id: alias-dictionary
url: search/nodejs-java/alias-dictionary
title: Alias dictionary
weight: 1
description: "This article gives the knowledge of the API methods which can be used to perform operations about Alias dictionary in Java."
keywords: Alias dictionary
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
An instance of the [AliasDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/AliasDictionary) class contains all the aliases defined in an index. Information on using aliases to search is provided on the [Using aliases]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/using-aliases.md" >}}) page.

To get the number of existing aliases, use the [getCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/AliasDictionary#getCount()) method.

Use the [add](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/AliasDictionary#add(java.lang.String,%20java.lang.String)) method to add new alias-replacement pair.

Use the [addRange](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/AliasDictionary#addRange(com.groupdocs.search.dictionaries.AliasReplacementPair%5B%5D)) method to add a collection of new alias-replacement pairs.

To remove an alias from the dictionary, use the [remove](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/AliasDictionary#remove(java.lang.String)) method.

The [contains](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/AliasDictionary#contains(java.lang.String)) method is used to check for the presence of a particular alias in the dictionary.

To get a replacement for a particular alias, use the [getText](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/AliasDictionary#getText(java.lang.String)) method.

The [clear](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/AliasDictionary#clear()) method is used to remove all existing aliases from the dictionary.

To export the list of aliases with replacements to a file, use the [exportDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#exportDictionary(java.lang.String)) method.

To import the list of aliases from a file, use the [importDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#importDictionary(java.lang.String)) method.

The following example demonstrates the use of methods of the alias dictionary.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating or opening an index from the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

if (index.getDictionaries().getAliasDictionary().getCount() > 0) {
  // Deleting all existing aliases
  index.getDictionaries().getAliasDictionary().clear();
}

// Adding aliases to the alias dictionary
index.getDictionaries().getAliasDictionary().add('t', '(gravida OR promotion)');
index.getDictionaries().getAliasDictionary().add('e', '(viverra OR farther)');
const pairs = java.newArray('com.groupdocs.search.dictionaries.AliasReplacementPair', [
  new groupdocs.search.AliasReplacementPair('d', 'daterange(2017-01-01 ~~ 2019-12-31)'),
  new groupdocs.search.AliasReplacementPair('n', '(400 ~~ 4000)'),
]);
index.getDictionaries().getAliasDictionary().addRange(pairs);

if (index.getDictionaries().getAliasDictionary().contains('e')) {
  // Getting an alias replacement
  const replacement = index.getDictionaries().getAliasDictionary().getText('e');
  console.log('e - ' + replacement);
}

// Export aliases to a file
const fileName = Utils.OutputPath + 'AdvancedUsage/ManagingDictionaries/aliasDictionary/Aliases.dat';
index.getDictionaries().getAliasDictionary().exportDictionary(fileName);

// Import aliases from a file
index.getDictionaries().getAliasDictionary().importDictionary(fileName);

// Search in the index
const query = '@t OR @e';
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
