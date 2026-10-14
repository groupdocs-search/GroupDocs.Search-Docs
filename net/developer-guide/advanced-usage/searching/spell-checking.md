---
id: spell-checking
url: search/net/spell-checking
title: Spell checking
weight: 26
description: "This article describes how spell checking works during the search."
keywords: Spell checking
productName: GroupDocs.Search for .NET
hideChildren: False
---
During the search, a search query can be checked for spelling errors in words. To enable spelling correction in search queries, the [Enabled](https://reference.groupdocs.com/search/net/groupdocs.search.options/spellingcorrectoroptions/enabled/) property of the spelling corrector options is set to true. By default, the spelling correction is disabled.

The spelling corrector considers the following types of misprints: adding a character, deleting a character, replacing a character, and, optionally, transposing two adjacent characters.

By default, the spelling corrector dictionary contains only English words. To manage the spelling corrector dictionary, see the [Spelling corrector]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/spelling-corrector.md" >}}) page of the [Managing dictionaries]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The [SpellingCorrectorOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/spellingcorrectoroptions/) class contains the following properties:

*   [MaxMistakeCount](https://reference.groupdocs.com/search/net/groupdocs.search.options/spellingcorrectoroptions/maxmistakecount/) is the maximum number of mistakes possible in each word of a search query. The default value is 2. A value greater than 10 is treated as 10, and a negative value is treated as 0.
*   [OnlyBestResults](https://reference.groupdocs.com/search/net/groupdocs.search.options/spellingcorrectoroptions/onlybestresults/) is a value indicating whether only the best results will be returned by the spelling corrector. The default value is false.
*   [OnlyBestResultsRange](https://reference.groupdocs.com/search/net/groupdocs.search.options/spellingcorrectoroptions/onlybestresultsrange/) is the maximum exceeding of the minimum number of mistakes that are found by the spelling corrector. The default value is 0.
*   [ConsiderTranspositions](https://reference.groupdocs.com/search/net/groupdocs.search.options/spellingcorrectoroptions/considertranspositions/) is a value indicating whether the spelling corrector must consider transposition of two adjacent characters as a single mistake (true) or two mistakes (false). The default value is true.

Spelling correction cannot be combined with case-sensitive search. If both are enabled, the search returns an empty result and the error is reported through the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event (see [Search options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}})).

Since version 26.10, spelling correction uses as many threads as there are processor cores by default. Therefore, the order of the found words in the search result may differ from run to run. To get the same order every time, set the [SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/) index setting to NumberOfThreads.One.

The following example shows how to perform a search using the spelling correction.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Creating a search options instance
SearchOptions options = new SearchOptions();
options.SpellingCorrector.Enabled = true; // Enabling the spelling correction
options.SpellingCorrector.MaxMistakeCount = 1; // Setting the maximum number of mistakes
options.SpellingCorrector.OnlyBestResults = true; // Enabling the option for only the best results of the spelling correction
 
// Search for the word "Rleativity" containing a spelling error
// The word "Relativity" will be found that differs from the search query in two transposed letters
SearchResult result = index.Search("Rleativity", options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
