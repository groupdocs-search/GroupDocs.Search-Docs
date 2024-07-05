---
id: keyboard-layout-correction
url: search/nodejs-java/keyboard-layout-correction
title: Keyboard layout correction
weight: 10
description:  "This article gives the knowledge that the keyboard layout correction feature can be used when entering search queries, users of your software may make input errors, forgetting to switch the desired keyboard layout using Java search API."
keywords: Keyboard layout correction
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
When entering search queries, users of your software may make input errors, forgetting to switch the desired keyboard layout. For example, entering the word 'Einstein' in the Russian keyboard layout will result in the word 'Уштыеушт' appearing.

To automatically fix such misprints, the keyboard layout correction feature can be used. To enable this feature, you must pass the true value as an argument to the [setEnabled](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/KeyboardLayoutCorrectorOptions#setEnabled(boolean)) method in the search options. By default, this feature is disabled.

The following example demonstrates using of the keyboard layout correction feature.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentsFolder);

// Creating a search options object
const options = new groupdocs.search.SearchOptions();
options.getKeyboardLayoutCorrector().setEnabled(true); // Enabling keyboard layout correction

// Search for word 'ызщкеыьфт' gives documents containing word 'sportsman'
const query = 'ызщкеыьфт';
const result = index.search(query, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
