---
id: stop-word-dictionary
url: search/net/stop-word-dictionary
title: Stop word dictionary
weight: 7
description: "This article describes the API for managing the stop word dictionary of an index."
keywords: stop word dictionary, stop words
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [StopWordDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/stopworddictionary/) class is designed to store stop words in an index. Stop words are excluded from indexing only if the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) index setting is enabled. Information on using stop words during indexing is provided on the [Indexing with stop words]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-with-stop-words.md" >}}) page.

{{< alert style="info" >}}Stop words define which terms of documents are indexed. Therefore, the stop word dictionary and the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) setting of an index can be changed only while the index contains no documents and is not performing an operation; otherwise, a change throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception). A call that does not change the content of the dictionary is allowed. Configure stop words after creating a new index and before adding documents. See also the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

The dictionary keeps words in lower case without leading and trailing white space. Therefore, the letter case of words does not matter when words are added, removed or checked.

To get the number of stop words in the dictionary, use the [Count](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/stopworddictionary/count/) property.

To add stop words to the dictionary, use the [AddRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/stopworddictionary/addrange/) method.

To remove words from the dictionary, use the [RemoveRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/stopworddictionary/removerange/) method.

To check for a word in the dictionary, use the [Contains](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/stopworddictionary/contains/) method. The method ignores the letter case of the word.

To remove all words from the dictionary, use the [Clear](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/stopworddictionary/clear/) method.

To export words to a file, use the [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) method.

To import words from a file, use the [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) method. Errors of exporting and importing are reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the stop word dictionary.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index settings instance
IndexSettings settings = new IndexSettings();
settings.UseStopWords = true; // Enabling the use of stop words during indexing

// Creating a new index in the specified folder; the dictionary can be changed only while the index contains no documents
Index index = new Index(indexFolder, settings, true);

if (index.Dictionaries.StopWordDictionary.Count > 0)
{
    // Removing all words from the dictionary
    index.Dictionaries.StopWordDictionary.Clear();
}

// Adding stop words to the dictionary
string[] words = new string[] { "a", "an", "the", "but", "by" };
index.Dictionaries.StopWordDictionary.AddRange(words);

// The letter case of words is ignored
if (index.Dictionaries.StopWordDictionary.Contains("But") &&
    index.Dictionaries.StopWordDictionary.Contains("BY"))
{
    // Removing words from the dictionary
    index.Dictionaries.StopWordDictionary.RemoveRange(new string[] { "but", "by" });
}

// Exporting words to a file
index.Dictionaries.StopWordDictionary.ExportDictionary(@"c:\StopWords.txt");

// Importing words from a file
index.Dictionaries.StopWordDictionary.ImportDictionary(@"c:\StopWords.txt");

// Indexing documents; after that, the stop words of the index cannot be changed
index.Add(documentsFolder);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
