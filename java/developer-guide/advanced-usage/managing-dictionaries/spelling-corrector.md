---
id: spelling-corrector
url: search/java/spelling-corrector
title: Spelling corrector
weight: 6
description: "This article gives the knowledge of the API methods which can be used to perform operations about spelling corrector using Java."
keywords: Spelling corrector
productName: GroupDocs.Search for Java
hideChildren: False
---
The [SpellingCorrector](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SpellingCorrector) class is designed to correct spelling errors in search queries, as well as to store words with correct spelling. You can learn about spelling correction in search queries on the [Spell checking]({{< ref "search/java/developer-guide/advanced-usage/searching/spell-checking.md" >}}) page.

To get the number of words in the spelling corrector dictionary, use the [getCount](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SpellingCorrector#getCount()) method.

To get an array of words containing in the spelling corrector dictionary, the [getWords](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SpellingCorrector#getWords()) method is used.

To add words to the spelling corrector dictionary, use the [addRange](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SpellingCorrector#addRange(java.lang.Iterable)) method.

The [clear](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/SpellingCorrector#clear()) method is used to remove all words from the spelling corrector dictionary.

To export words to a file, use the [exportDictionary](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/DictionaryBase#exportDictionary(java.lang.String)) method.

To import words from a file, use the [importDictionary](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/DictionaryBase#importDictionary(java.lang.String)) method.

The following example demonstrates the use of methods of the spelling corrector.



```java
String indexFolder = "c:\\MyIndex\\";
 
// Creating an index from in specified folder
Index index = new Index(indexFolder);
 
if (index.getDictionaries().getSpellingCorrector().getCount() > 0) {
    // Removing all words from the dictionary
    index.getDictionaries().getSpellingCorrector().clear();
}
 
// Adding words to the dictionary
String[] words = new String[] { "achieve", "accomplish", "attain", "reach" };
index.getDictionaries().getSpellingCorrector().addRange(words);
 
// Export words to a file
index.getDictionaries().getSpellingCorrector().exportDictionary("C:\\Words.txt");
 
// Import words from a file
index.getDictionaries().getSpellingCorrector().importDictionary("C:\\Words.txt");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
