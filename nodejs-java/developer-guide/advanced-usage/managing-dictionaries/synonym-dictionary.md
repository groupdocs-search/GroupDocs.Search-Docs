---
id: synonym-dictionary
url: search/nodejs-java/synonym-dictionary
title: Synonym dictionary
weight: 8
description: "This article gives the knowledge of the API methods which can be used to perform operations about Synonym dictionary using Java."
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
The [SynonymDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/SynonymDictionary) class is designed to store synonyms in an index. For information on searching using synonyms, see the [Synonym search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/synonym-search.md" >}}) page.

To get the number of synonyms in the dictionary, use the [getCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/SynonymDictionary#getCount()) method.

To add groups of synonyms to the dictionary, use the [addRange](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/SynonymDictionary#addRange(java.lang.Iterable)) method.

The [clear](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/SynonymDictionary#clear()) method is used to remove all synonyms from the dictionary.

The [getSynonyms](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/SynonymDictionary#getSynonyms(java.lang.String)) method is used to get a list of synonyms for a given word.

The [getSynonymGroups](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/SynonymDictionary#getSynonymGroups(java.lang.String)) method is used to get all synonym groups to which a given word belongs.

To get all synonym groups from the dictionary, use the [getAllSynonymGroups](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/SynonymDictionary#getAllSynonymGroups()) method.

To export synonyms to a file, use the [exportDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#exportDictionary(java.lang.String)) method.

To import synonyms from a file, use the [importDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#importDictionary(java.lang.String)) method.

The following example demonstrates the use of methods of the synonym dictionary.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index from in specified folder
const index = new groupdocs.search.Index(indexFolder, true);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Getting synonyms for word 'make'
const synonyms = index.getDictionaries().getSynonymDictionary().getSynonyms('make');
console.log("Synonyms for 'make':");
for (const synonym of synonyms) {
  console.log(synonym);
}

// Getting groups of synonyms to which word 'make' belongs to
const groups = index.getDictionaries().getSynonymDictionary().getSynonymGroups('make');
console.log("Synonym groups for 'make':");
for (const group of groups) {
  for (const group1 of group) {
    console.log(group1 + ' ');
  }
  console.log();
}

if (index.getDictionaries().getSynonymDictionary().getCount() > 0) {
  // Removing all synonyms from the dictionary
  index.getDictionaries().getSynonymDictionary().clear();
}

// Adding synonyms to the dictionary
const synonymGroups = java.newInstanceSync('java.util.ArrayList');
synonymGroups.add(java.newArray('java.lang.String', ['achieve', 'accomplish', 'attain', 'reach']));
synonymGroups.add(java.newArray('java.lang.String', ['achieve', 'accept', 'take', 'have']));
synonymGroups.add(java.newArray('java.lang.String', ['improve', 'better']));
index.getDictionaries().getSynonymDictionary().addRange(synonymGroups);

// Export synonyms to a file
const fileName = Utils.OutputPath + 'AdvancedUsage/ManagingDictionaries/synonymDictionary/Synonyms.dat';
index.getDictionaries().getSynonymDictionary().exportDictionary(fileName);

// Import synonyms from a file
index.getDictionaries().getSynonymDictionary().importDictionary(fileName);

// Search in the index
const query = 'achieve';
const options = new groupdocs.search.SearchOptions();
options.setUseSynonymSearch(true); // Enabling synonym search
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
