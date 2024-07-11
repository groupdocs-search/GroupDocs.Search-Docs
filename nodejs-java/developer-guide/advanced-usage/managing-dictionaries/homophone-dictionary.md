---
id: homophone-dictionary
url: search/nodejs-java/homophone-dictionary
title: Homophone dictionary
weight: 5
description: "This article gives the knowledge of the API methods which can be used to perform operations about homophone dictionary using Java."
keywords: Homophone dictionary
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
The [HomophoneDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/HomophoneDictionary) class is designed to store homophones in an index. For information on searching using homophones, see the [Homophone search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/homophone-search.md" >}}) page.

To get the number of homophones in the dictionary, use the [getCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/HomophoneDictionary#getCount()) method.

To add groups of homophones to the dictionary, use the [addRange](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/HomophoneDictionary#addRange(java.lang.Iterable)) method.

The [clear](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/HomophoneDictionary#clear()) method is used to remove all homophones from the dictionary.

The [getHomophones](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/HomophoneDictionary#getHomophones(java.lang.String)) method is used to get a list of synonyms for a given word.

The [getHomophoneGroups](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/HomophoneDictionary#getHomophoneGroups(java.lang.String)) method is used to get all synonym groups to which a given word belongs.

To get all synonym groups from the dictionary, use the [getAllHomophoneGroups](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/HomophoneDictionary#getAllHomophoneGroups()) method.

To export homophones to a file, use the [exportDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#exportDictionary(java.lang.String)) method.

To import homophones from a file, use the [importDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#importDictionary(java.lang.String)) method.

The following example demonstrates the use of methods of the homophone dictionary.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index from in specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Getting homophones for word 'braid'
const homophones = index.getDictionaries().getHomophoneDictionary().getHomophones('braid');
console.log("Homophones for 'braid':");
for (const homophone of homophones) {
  console.log(homophone);
}

// Getting groups of homophones to which word 'braid' belongs to
const groups = index.getDictionaries().getHomophoneDictionary().getHomophoneGroups('braid');
console.log("Homophone groups for 'braid':");
for (const group of groups) {
  for (const group1 of group) {
    console.log(group1 + ' ');
  }
  console.log();
}

if (index.getDictionaries().getHomophoneDictionary().getCount() > 0) {
  // Removing all homophones from the dictionary
  index.getDictionaries().getHomophoneDictionary().clear();
}

// Adding homophones to the dictionary
const homophoneGroups = java.newInstanceSync('java.util.ArrayList');
homophoneGroups.add(java.newArray('java.lang.String', ['awe', 'oar', 'or', 'ore']));
homophoneGroups.add(java.newArray('java.lang.String', ['aye', 'eye', 'i']));
homophoneGroups.add(java.newArray('java.lang.String', ['call', 'caul']));
index.getDictionaries().getHomophoneDictionary().addRange(homophoneGroups);

// Export homophones to a file
const fileName = Utils.OutputPath + 'AdvancedUsage/ManagingDictionaries/homophoneDictionary/Homophones.dat';
index.getDictionaries().getHomophoneDictionary().exportDictionary(fileName);

// Import homophones from a file
index.getDictionaries().getHomophoneDictionary().importDictionary(fileName);

// Search in the index
const query = 'caul';
const options = new groupdocs.search.SearchOptions();
options.setUseHomophoneSearch(true);
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
