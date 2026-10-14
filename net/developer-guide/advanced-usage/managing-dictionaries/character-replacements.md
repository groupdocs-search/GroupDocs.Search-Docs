---
id: character-replacements
url: search/net/character-replacements
title: Character replacements
weight: 3
description: "This article describes the API for managing the character replacement dictionary of an index."
keywords: character replacements, character replacement dictionary
productName: GroupDocs.Search for .NET
hideChildren: False
---
An instance of the [CharacterReplacementDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/characterreplacementdictionary/) class contains all the character replacements defined in an index. Character replacements are applied during indexing only if the [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/) index setting is enabled. For detailed information on character replacement, see the [Character replacement during indexing]({{< ref "search/net/developer-guide/advanced-usage/indexing/character-replacement-during-indexing.md" >}}) page.

{{< alert style="info" >}}Character replacements define how the text of documents is divided into terms. Therefore, the character replacement dictionary and the [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/) setting of an index can be changed only while the index contains no documents and is not performing an operation; otherwise, a change throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception). A call that does not change the content of the dictionary is allowed. Configure character replacements after creating a new index and before adding documents. See also the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

The [Count](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/characterreplacementdictionary/count/) property allows you to get the number of character replacements defined in the dictionary.

To add character replacements to the dictionary, use the [AddRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/characterreplacementdictionary/addrange/) method. The method accepts a collection of [CharacterReplacementPair](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/characterreplacementpair/) objects or a collection of key-value pairs. A pair can be created from two *char* values or from two Unicode code points (*int* values); the latter allows you to define replacements for characters outside the Basic Multilingual Plane. A replacement added for a character that already has one replaces the previous replacement. A pair whose replacement is the same as the character removes the replacement of the character.

To remove character replacements from the dictionary, the [RemoveRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/characterreplacementdictionary/removerange/) method is used. The characters can be specified as *char* values or as Unicode code points.

The [Contains](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/characterreplacementdictionary/contains/) method is used to determine if the dictionary contains a replacement for the specified character.

To get a replacement for the specified character, use the [GetReplacement](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/characterreplacementdictionary/getreplacement/) method. The method returns the Unicode code point of the replacement, or the specified character itself if the dictionary contains no replacement for it.

To remove all replacements from the dictionary, use the [Clear](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/characterreplacementdictionary/clear/) method.

To export all replacements to a file, use the [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) method.

To import character replacements from a file, use the [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) method. Errors of exporting and importing are reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index instead of exceptions.

The following example demonstrates the use of the character replacement dictionary methods.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index settings instance
IndexSettings settings = new IndexSettings();
settings.UseCharacterReplacements = true; // Enabling character replacements during indexing

// Creating a new index in the specified folder; the dictionary can be changed only while the index contains no documents
Index index = new Index(indexFolder, settings, true);

if (index.Dictionaries.CharacterReplacements.Contains('-'))
{
    // Getting the replacement for the hyphen character
    int replacement = index.Dictionaries.CharacterReplacements.GetReplacement('-');
    Console.WriteLine("The replacement for hyphen is " + char.ConvertFromUtf32(replacement));

    // Deleting the hyphen character replacement from the dictionary
    index.Dictionaries.CharacterReplacements.RemoveRange(new char[] { '-' });
}

if (index.Dictionaries.CharacterReplacements.Count > 0)
{
    // Deleting all character replacements from the dictionary
    index.Dictionaries.CharacterReplacements.Clear();
}

// Adding character replacements to the dictionary
CharacterReplacementPair[] pairs = new CharacterReplacementPair[]
{
    new CharacterReplacementPair('Ä', 'a'),
    new CharacterReplacementPair('ä', 'a'),
    new CharacterReplacementPair(0x1D400, 0x0061), // Mathematical bold capital A is replaced with the small letter a
};
index.Dictionaries.CharacterReplacements.AddRange(pairs);

// Exporting character replacements to a file
index.Dictionaries.CharacterReplacements.ExportDictionary(@"c:\CharacterReplacements.dat");

// Importing character replacements from a file
index.Dictionaries.CharacterReplacements.ImportDictionary(@"c:\CharacterReplacements.dat");

// Indexing documents; after that, the character replacements of the index cannot be changed
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
