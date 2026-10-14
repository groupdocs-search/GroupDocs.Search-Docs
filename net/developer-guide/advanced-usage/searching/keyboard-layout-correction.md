---
id: keyboard-layout-correction
url: search/net/keyboard-layout-correction
title: Keyboard layout correction
weight: 10
description: "This article describes the keyboard layout correction feature, which fixes words in search queries typed in a wrong keyboard layout."
keywords: Keyboard layout correction
productName: GroupDocs.Search for .NET
hideChildren: False
---
When entering search queries, users of your software may make input errors, forgetting to switch the desired keyboard layout. For example, entering the word 'Einstein' in the Russian keyboard layout will result in the word 'Уштыеушт' appearing.

To automatically fix such misprints, the keyboard layout correction feature can be used. To enable this feature, you must set the true value for the [Enabled](https://reference.groupdocs.com/search/net/groupdocs.search.options/keyboardlayoutcorrectoroptions/enabled/) property in the search options. By default, this feature is disabled.

Keyboard layout correction cannot be combined with case-sensitive search. If both are enabled, the search returns an empty result and the error is reported through the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event (see [Search options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}})).

The following example demonstrates the use of the keyboard layout correction feature.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Creating a search options object
SearchOptions options = new SearchOptions();
options.KeyboardLayoutCorrector.Enabled = true; // Enabling keyboard layout correction

// Search for the word 'Уштыеушт' gives documents containing the word 'Einstein'
SearchResult result = index.Search("Уштыеушт", options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
