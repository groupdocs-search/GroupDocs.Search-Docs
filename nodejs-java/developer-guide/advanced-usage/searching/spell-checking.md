---
id: spell-checking
url: search/nodejs-java/spell-checking
title: Spell checking
weight: 26
description: "This article shows that how spell checking works during the search."
keywords: Spell checking
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
During the search, a search query can be checked for spelling errors in words. To enable spelling correction in search queries, the [setEnabled](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SpellingCorrectorOptions#setEnabled(boolean)) method of the spelling corrector options is called with the true value as an argument. By default, the spelling correction is disabled.

The spelling corrector considers the following types of misprints: adding a character, deleting a character, replacing a character, and, optionally, transposing two adjacent characters.

By default, the spelling corrector dictionary contains only English words. To manage the spelling corrector dictionary, see the [Spelling corrector]({{< ref "search/nodejs-java/developer-guide/advanced-usage/managing-dictionaries/spelling-corrector.md" >}}) page of the [Managing dictionaries]({{< ref "search/nodejs-java/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The spelling corrector options class contains the following options:

*   [setMaxMistakeCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SpellingCorrectorOptions#setMaxMistakeCount(int)) sets the maximum number of mistakes possible in each word of a search query. The default value is 2.
*   [setOnlyBestResults](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SpellingCorrectorOptions#setOnlyBestResults(boolean)) sets a value indicating whether only the best results will be returned by the spelling corrector. The default value is false.
*   [setOnlyBestResultsRange](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SpellingCorrectorOptions#setOnlyBestResultsRange(byte)) sets the maximum exceeding of the minimum number of mistakes that are found by the spelling corrector. The default value is 0.
*   [setConsiderTranspositions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SpellingCorrectorOptions#setConsiderTranspositions(boolean)) sets a value indicating whether the spelling corrector must consider transposition of two adjacent characters as a single mistake (true) or two mistakes (false). The default value is true.

The following example shows how to perform a search using the spelling correction.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Creating a search options instance
const options = new groupdocs.search.SearchOptions();
options.getSpellingCorrector().setEnabled(true); // Enabling the spelling correction
options.getSpellingCorrector().setMaxMistakeCount(1); // Setting the maximum number of mistakes
options.getSpellingCorrector().setOnlyBestResults(true); // Enabling the option for only the best results of the spelling correction

// Search for the word "houseohld" containing a spelling error
// The word "household" will be found that differs from the search query in two transposed letters
const query = 'houseohld';
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
