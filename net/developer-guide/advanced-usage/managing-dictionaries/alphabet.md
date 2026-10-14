---
id: alphabet
url: search/net/alphabet
title: Alphabet
weight: 2
description: "This article describes the API for managing the alphabet of an index, which defines the types of characters used to divide text into words."
keywords: alphabet, character types, separator, letter, blended character, surrogate pair
productName: GroupDocs.Search for .NET
hideChildren: False
---
An instance of the [Alphabet](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/) class contains the types of characters used for indexing. The alphabet of an index is available through the [Alphabet](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/alphabet/) property of the [Index.Dictionaries](https://reference.groupdocs.com/search/net/groupdocs.search/index/dictionaries/) object. For detailed information on the types of characters and the default content of the alphabet, see the [Character types]({{< ref "search/net/developer-guide/advanced-usage/indexing/character-types.md" >}}) page.

{{< alert style="info" >}}The alphabet defines how the text of documents is divided into terms. Therefore, the alphabet of an index can be changed only while the index contains no documents and is not performing an operation; otherwise, a method that changes the alphabet throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception). A call that does not change the content of the alphabet is allowed. Configure the alphabet after creating a new index and before adding documents. See also the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

The [GetCharacterType](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/getcharactertype/) method is used to get the type of a specific character. The method has three overloads: the character can be specified as a *char* value, as a Unicode code point (an *int* value), or as a string that contains a character or a surrogate pair (a character outside the Basic Multilingual Plane).

The [Count](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/count/) property returns the number of characters of a type other than [CharacterType](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/charactertype/).Separator. Enumerating an [Alphabet](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/) object returns the Unicode code points of these characters.

To set the type of characters in the alphabet, use the [SetRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/setrange/) method. The characters can be passed as an array of *char* values, as an array of Unicode code points, or as an array of strings, each containing a character or a surrogate pair. The last two overloads allow you to set the type of characters outside the Basic Multilingual Plane.

To set the [CharacterType](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/charactertype/).Separator type for all characters in the alphabet, use the [Clear](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/clear/) method. Note that after clearing, the alphabet contains no letters, so no words can be indexed. Call this method only when you are going to define all the required letters afterwards, for example, with the [SetRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/setrange/) method or by importing an alphabet from a file.

To export types of all characters to a file, use the [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) method.

To import types of characters from a file, use the [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) method. The imported types are set for the characters listed in the file; the types of other characters are not changed. To make the alphabet exactly the same as the exported one, call the [Clear](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/clear/) method before importing. Errors of exporting and importing are reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the alphabet.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating a new index in the specified folder; the alphabet can be changed only while the index contains no documents
Index index = new Index(indexFolder, true);

// Getting the type of the hyphen character
CharacterType hyphenType = index.Dictionaries.Alphabet.GetCharacterType('-');
Console.WriteLine("The type of hyphen: " + hyphenType);

if (hyphenType != CharacterType.Blended)
{
    // Setting the Blended type for the hyphen character
    index.Dictionaries.Alphabet.SetRange(new char[] { '-' }, CharacterType.Blended);
}

// Setting the Letter type for characters outside the Basic Multilingual Plane specified as Unicode code points
index.Dictionaries.Alphabet.SetRange(new int[] { 0x1D400, 0x1D401 }, CharacterType.Letter); // Mathematical bold capital A and B

// Setting the Letter type for a character specified as a surrogate pair
index.Dictionaries.Alphabet.SetRange(new string[] { "\U0001D41A" }, CharacterType.Letter); // Mathematical bold small A

// Getting the types of characters specified as a Unicode code point and as a surrogate pair
Console.WriteLine("The type of U+1D400: " + index.Dictionaries.Alphabet.GetCharacterType(0x1D400));
Console.WriteLine("The type of U+1D41A: " + index.Dictionaries.Alphabet.GetCharacterType("\U0001D41A"));

// Getting the number of characters that are not separators
Console.WriteLine("Characters that are not separators: " + index.Dictionaries.Alphabet.Count);

// Exporting the alphabet to a file
index.Dictionaries.Alphabet.ExportDictionary(@"c:\Alphabet.dat");

// Indexing documents; after that, the alphabet of the index cannot be changed
index.Add(documentsFolder);
```

The following example demonstrates how to create another index with the same alphabet by importing the alphabet exported in the previous example.

**C#**

```csharp
string newIndexFolder = @"c:\MyNewIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating a new index in the specified folder
Index newIndex = new Index(newIndexFolder, true);

// Setting the Separator type for all characters, so that only the imported types remain in the alphabet
newIndex.Dictionaries.Alphabet.Clear();

// Importing the alphabet from a file
newIndex.Dictionaries.Alphabet.ImportDictionary(@"c:\Alphabet.dat");

// Indexing documents
newIndex.Add(documentsFolder);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
