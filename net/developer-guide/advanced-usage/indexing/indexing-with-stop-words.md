---
id: indexing-with-stop-words
url: search/net/indexing-with-stop-words
title: Indexing with stop words
weight: 18
description: "This article explains how to enable or disable the use of stop words during indexing."
keywords: stop words, indexing with stop words, UseStopWords, stop word dictionary
productName: GroupDocs.Search for .NET
hideChildren: False
---
Stop words are frequently used words that do not carry a semantic meaning and can be removed from an index to reduce its size.

You can enable or disable the use of stop words by setting a value of the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) property of the [IndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/) class. The default value is true, meaning that stop words are filtered during indexing and not added to an index.

A list of stop words to use during indexing can be specified in the stop word dictionary. By default, the stop word dictionary is filled with the most widely used pronouns, prepositions and conjunctions of English and Russian. The list of stop words can be replaced or supplemented, and it is saved in the index. Stop words are kept in lower case, and the lookups in the stop word dictionary ignore letter case. For information on managing the stop word dictionary, see the [Stop word dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary.md" >}}) page in the [Managing dictionaries]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

Since version 26.10, the stop word dictionary and the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) setting can be changed only while the index contains no documents and is not performing an operation; otherwise, [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) is thrown. Therefore, configure them after creating a new index and before adding the first document, as the example below does. To use other stop words with documents that are already indexed, create a new index and index the documents again. See the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page for details.

If you need to keep all text information extracted from documents, and you are not afraid of a significant increase in the size of the index, then an example of indexing without stop words can be found below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating index settings with the use of stop words disabled
IndexSettings settings = new IndexSettings();
settings.UseStopWords = false;
 
// Creating a new index in the specified folder
// The UseStopWords setting can be changed only while the index contains no documents
Index index = new Index(indexFolder, settings, true);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Searching in the index
// Now in the index it is possible to search for the stop word 'on'
SearchResult result = index.Search("on");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
