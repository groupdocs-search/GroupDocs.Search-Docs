---
id: managing-dictionaries
url: search/net/managing-dictionaries
title: Managing Dictionaries
weight: 4
description: "This section describes the dictionaries of a search index and the API for managing them."
keywords: dictionaries, alphabet, tokenization dictionary, character replacements, stop words, synonyms, homophones, aliases, spelling corrector, document passwords
productName: GroupDocs.Search for .NET
hideChildren: False
---
Each index has a set of dictionaries that are used during indexing and searching. The dictionaries are available through the [Dictionaries](https://reference.groupdocs.com/search/net/groupdocs.search/index/dictionaries/) property of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class, which returns an instance of the [DictionaryRepository](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/) class. The dictionaries of an index stored on disk are saved in the index folder, and every change of a dictionary is saved immediately.

The articles in this section describe the following dictionaries:

*   [Alphabet]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}) — the types of characters that define how text is divided into words.
*   [Tokenization dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary.md" >}}) — the types of characters that define which words returned by a custom text segmenter are indexed.
*   [Character replacements]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/character-replacements.md" >}}) — the replacements of characters applied during indexing.
*   [Stop word dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary.md" >}}) — the words that are not indexed.
*   [Synonym dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/synonym-dictionary.md" >}}) — the groups of synonyms used in synonym search.
*   [Homophone dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/homophone-dictionary.md" >}}) — the groups of homophones used in homophone search.
*   [Alias dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alias-dictionary.md" >}}) — the aliases that can be used in search queries.
*   [Spelling corrector]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/spelling-corrector.md" >}}) — the words with correct spelling used in spell checking.
*   [Document passwords]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/document-passwords.md" >}}) — the passwords of protected documents to be indexed.
*   [Word forms provider]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/word-forms-provider.md" >}}) — the provider of word forms used in search for different word forms.

## When dictionaries can be changed

The alphabet, the tokenization dictionary, the character replacements and the stop word dictionary, as well as the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) and [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/) index settings, define how the text of documents is divided into terms. Therefore, they can be changed only while the index contains no documents and is not performing an operation (its status is [IndexStatus.Ready](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexstatus/)). Otherwise, a method or a property setter that changes them throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception). A call that does not change the content of a dictionary or the value of a setting is allowed at any time. Configure these dictionaries and settings after creating a new index and before adding the first document. To use another content with documents that are already indexed, create a new index and index the documents again.

The synonym dictionary, the homophone dictionary, the alias dictionary, the spelling corrector and the document passwords can be changed at any time. The word forms provider can also be replaced at any time.

For more information on the dictionaries and settings that are fixed in an index, see the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

## Exporting and importing dictionaries

Each dictionary class is derived from the [DictionaryBase](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/) class, which provides the [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) and [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) methods for saving the content of a dictionary to a file and loading it from a file.

These methods do not throw an exception if the file cannot be written or read, or if it has an invalid format. Instead, the error is reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the [Index.Events](https://reference.groupdocs.com/search/net/groupdocs.search/index/events/) object, and a dictionary of the index is not changed. However, the [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if the import would change a dictionary that cannot be changed in the index, as described above.

The following example demonstrates how to handle errors of exporting and importing dictionaries.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Creating or opening an index in the specified folder
Index index = new Index(indexFolder);

// Subscribing to the event that reports errors, including errors of exporting and importing dictionaries
index.Events.ErrorOccurred += (sender, args) =>
{
    Console.WriteLine("Error: " + args.Message);
};

// Exporting synonyms to a file
index.Dictionaries.SynonymDictionary.ExportDictionary(@"c:\Synonyms.dat");

// Importing synonyms from a file; if the file does not exist, the error is reported by the ErrorOccurred event
index.Dictionaries.SynonymDictionary.ImportDictionary(@"c:\MissingFile.dat");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
