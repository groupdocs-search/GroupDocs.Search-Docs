---
id: character-types
url: search/net/character-types
title: Character types
weight: 2
description: "This page contains descriptions of all character types. Character types differ in how characters of these types are indexed."
keywords: character types, alphabet, separator, letter, blended characters, separate word, CharacterType
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains descriptions of all character types. Character types differ in how characters of these types are indexed. The type of each character is specified in the alphabet of an index, which is described by the [Alphabet](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/) class. Character types are represented by the [CharacterType](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/charactertype/) enumeration: **Separator**, **Letter**, **Blended** and **SeparateWord**. For more information on managing the alphabet, see the [Alphabet]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}) page in the [Managing dictionaries]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The alphabet defines how the text of documents is divided into words, so it can be changed only while the index contains no documents and is not performing an operation. Otherwise, a method that changes the alphabet throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception). Therefore, configure the alphabet after creating a new index and before adding the first document, as the examples below do. To use another alphabet with documents that are already indexed, create a new index and index the documents again. See the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page for details.

## Regular characters

Regular characters are separators and letters. During indexing, separators are used to separate words from each other, and words are formed from continuous sequences of letters.

The type of each particular character can be specified in the alphabet. By default, the following characters are defined as letters (Unicode code points are given):

*   Digits: 0030-0039;
*   Latin capital letters: 0041-005A;
*   Low line: 005F;
*   Latin small letters: 0061-007A;
*   Latin letters: 00C0-00D6, 00D8-00F6, 00F8-00FF, 0100-017F, 0180-024F, 0250-02AF;
*   Greek and Coptic letters: 0370-0373, 0376-0377, 037F, 0386, 0388-038A, 038C, 038E-03A1, 03A3-03FC;
*   Cyrillic letters: 0400-0481, 048A-04FF, 0500-052F;
*   Armenian letters: 0531-0556, 0561-0587;
*   Arabic letters: 0610-061A, 0620-0669, 066E-06D3, 06D5-06DC, 06DF-06E8, 06EA-06FC, 06FF, 0750-077F, 0870-0887, 0889-088E, 0898-089F, 08A0-08E1, 08E3-08FF, FB50-FBB1, FBD3-FD3D, FD50-FD8F, FD92-FDC7, FDF0-FDFB, FE70-FE74, FE76-FEFC, 10EFD-10EFF, as well as the letters of the Arabic Mathematical Alphabetic Symbols block (1EE00-1EEBB).

The other characters are defined as separators in the alphabet.

The following example demonstrates how to specify only digits, the low line character, and English letters as letters in the alphabet.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating a new index in the specified folder
// The alphabet can be changed only while the index contains no documents
Index index = new Index(indexFolder, true);
 
// Configuring the alphabet
// Setting the separator type for all characters in the alphabet
index.Dictionaries.Alphabet.Clear();
// Creating a list of letter characters
List<char> list = new List<char>();
for (char i = (char)0x0030; i <= 0x0039; i++)
{
    list.Add(i); // Digits
}
for (char i = (char)0x0041; i <= 0x005A; i++)
{
    list.Add(i); // Latin capital letters
}
list.Add((char)0x005F); // Low line
for (char i = (char)0x0061; i <= 0x007A; i++)
{
    list.Add(i); // Latin small letters
}
// Setting the type of characters in the alphabet
char[] characters = list.ToArray();
index.Dictionaries.Alphabet.SetRange(characters, CharacterType.Letter);
 
// Indexing documents from the specified folder
index.Add(documentFolder);
 
// Searching in the index
SearchResult result = index.Search("Einstein");
```

The [SetRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/setrange/) method also has overloads that take an array of strings (each string is a character or a surrogate pair) and an array of Unicode code points, which allow you to specify characters outside the Basic Multilingual Plane.

## Blended characters

Blended characters are special characters that are indexed both as separators and as letters. This type of character can be useful, for example, for indexing hyphens. In this case, parts of a compound word containing a hyphen will be indexed both as a single word with a hyphen and individually without a hyphen. An example of using blended characters is presented below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating a new index in the specified folder
Index index = new Index(indexFolder, true);
 
// Setting hyphen character type to blended
index.Dictionaries.Alphabet.SetRange(new char[] { '-' }, CharacterType.Blended);
 
// Indexing documents from the specified folder
index.Add(documentFolder);
 
// Searching in the index
SearchResult result1 = index.Search("Elliot-Murray-Kynynmound");
SearchResult result2 = index.Search("Elliot");
SearchResult result3 = index.Search("Murray");
SearchResult result4 = index.Search("Kynynmound");
```

A sequence of blended characters without letters, for example a standalone hyphen, also forms a term. By default, such terms are not indexed, because the [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) index setting is enabled. To keep them in the index, disable this setting when creating the index (see [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}})).

## Indexing each character as a whole word

Another special type of character is a character indexed as a separate word, the **SeparateWord** value of the [CharacterType](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/charactertype/) enumeration. This type of character is designed to work with hieroglyphic languages and allows you to index each such character in the text as a separate word, regardless of the presence of separators. By default, no characters of this type are defined in the alphabet. As an alternative for such languages, you can split text into words with an external library, see the [Custom text segmenter]({{< ref "search/net/developer-guide/advanced-usage/indexing/custom-text-segmenter.md" >}}) page.

The following example demonstrates how to index each CJK unified ideograph as a separate word.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating a new index in the specified folder
Index index = new Index(indexFolder, true);
 
// Setting the separate word type for the CJK unified ideographs
List<char> list = new List<char>();
for (char i = (char)0x4E00; i <= 0x9FFF; i++)
{
    list.Add(i);
}
index.Dictionaries.Alphabet.SetRange(list.ToArray(), CharacterType.SeparateWord);
 
// Indexing documents from the specified folder
index.Add(documentFolder);
 
// Searching in the index for a single character
SearchResult result = index.Search("书");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
