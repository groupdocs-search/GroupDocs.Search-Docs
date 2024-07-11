---
id: stop-word-dictionary
url: search/nodejs-java/stop-word-dictionary
title: Stop word dictionary
weight: 7
description: "This article gives the knowledge of the API methods which can be used to perform operations about stop word dictionary using Java."
keywords: Stop word dictionary
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
The [StopWordDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/StopWordDictionary) class is designed to store stop words in an index. Information on using stop words during indexing is provided on the [Indexing with stop words]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/indexing-with-stop-words.md" >}}) page.

To get the number of stop words in the dictionary, use the [getCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/StopWordDictionary#getCount()) method.

To add stop words to the dictionary, use the [addRange](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/StopWordDictionary#addRange(java.lang.Iterable)) method.

To remove words from the dictionary, use the [removeRange](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/StopWordDictionary#removeRange(java.lang.Iterable)) method.

To check for a word in a dictionary, use the [contains](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/StopWordDictionary#contains(java.lang.String)) method.

To remove all words from the dictionary, use the [clear](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/StopWordDictionary#clear()) method.

To export words to a file, use the [exportDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#exportDictionary(java.lang.String)) method.

To import words from a file, use the [importDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/DictionaryBase#importDictionary(java.lang.String)) method.

The following example demonstrates the use of methods of the stop word dictionary.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index from in specified folder
const index = new groupdocs.search.Index(indexFolder);

if (index.getDictionaries().getStopWordDictionary().getCount() > 0) {
  // Removing all words from the dictionary
  index.getDictionaries().getStopWordDictionary().clear();
}

// Adding stop words to the dictionary
const words = java.newArray('java.lang.String', ['a', 'an', 'the', 'but', 'by']);
index.getDictionaries().getStopWordDictionary().addRange(words);

if (
  index.getDictionaries().getStopWordDictionary().contains('but') &&
  index.getDictionaries().getStopWordDictionary().contains('by')
) {
  // Removing words from the dictionary
  index
    .getDictionaries()
    .getStopWordDictionary()
    .removeRange(java.newArray('java.lang.String', ['but', 'by']));
}

// Export words to a file
const fileName = Utils.OutputPath + 'AdvancedUsage/ManagingDictionaries/stopWordDictionary/Words.txt';
index.getDictionaries().getStopWordDictionary().exportDictionary(fileName);

// Import words from a file
index.getDictionaries().getStopWordDictionary().importDictionary(fileName);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Search in the index
const query = 'but';
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
