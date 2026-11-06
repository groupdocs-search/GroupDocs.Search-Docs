---
id: spelling-corrector
url: search/net/spelling-corrector
title: Spelling corrector
weight: 6
description: "This article describes the API for managing the spelling corrector dictionary of an index."
keywords: spelling corrector, spell checking dictionary
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [SpellingCorrector](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/spellingcorrector/) class is designed to correct spelling errors in search queries, as well as to store words with correct spelling. You can learn about spelling correction in search queries on the [Spell checking]({{< ref "search/net/developer-guide/advanced-usage/searching/spell-checking.md" >}}) page.

The spelling corrector dictionary can be changed at any time, also in an index that contains documents.

To get the number of words in the spelling corrector dictionary, use the [Count](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/spellingcorrector/count/) property.

To get an array of words contained in the spelling corrector dictionary, the [GetWords](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/spellingcorrector/getwords/) method is used.

To add words to the spelling corrector dictionary, use the [AddRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/spellingcorrector/addrange/) method.

The [Clear](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/spellingcorrector/clear/) method is used to remove all words from the spelling corrector dictionary.

To export words to a file, use the [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) method.

To import words from a file, use the [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) method. Errors of exporting and importing are reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index instead of exceptions.

Since version 26.11, a word of the spelling corrector has from 1 to 80 characters and does not contain the characters U+0000, U+0001, tab, carriage return or line feed. The [AddRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/spellingcorrector/addrange/) method and an import skip the other words, for example, *null* or empty words and words longer than 80 characters, add the rest, and report the number of the skipped words by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event; an empty line of an imported file is skipped without an error. When an index is opened, the longer words of a spelling corrector dictionary saved by an earlier version are left out. A word of a search query that has more than 80 characters is not corrected.

The spelling corrector allows 10 mistakes in a word at most: a greater value of the [MaxMistakeCount](https://reference.groupdocs.com/search/net/groupdocs.search.options/spellingcorrectoroptions/maxmistakecount/) option is taken for 10 (see [Spell checking]({{< ref "search/net/developer-guide/advanced-usage/searching/spell-checking.md" >}})).

The following example demonstrates the use of methods of the spelling corrector.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Creating or opening an index in the specified folder
Index index = new Index(indexFolder);

if (index.Dictionaries.SpellingCorrector.Count > 0)
{
    // Removing all words from the dictionary
    index.Dictionaries.SpellingCorrector.Clear();
}

// Adding words to the dictionary
string[] words = new string[] { "achieve", "accomplish", "attain", "reach" };
index.Dictionaries.SpellingCorrector.AddRange(words);

// Getting all words of the dictionary
string[] allWords = index.Dictionaries.SpellingCorrector.GetWords();
Console.WriteLine("Words in the dictionary: " + allWords.Length);

// Exporting words to a file
index.Dictionaries.SpellingCorrector.ExportDictionary(@"c:\Words.txt");

// Importing words from a file
index.Dictionaries.SpellingCorrector.ImportDictionary(@"c:\Words.txt");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
