---
id: tokenization-dictionary
url: search/net/tokenization-dictionary
title: Tokenization dictionary
weight: 9
description: "This article describes the tokenization dictionary of an index, which defines the words returned by a custom text segmenter that are indexed."
keywords: tokenization dictionary, custom text segmenter, word splitter, IWordSplitter, CJK, hieroglyphic languages
productName: GroupDocs.Search for .NET
hideChildren: False
---
The tokenization dictionary of an index is available through the [TokenizationDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/tokenizationdictionary/) property of the [Index.Dictionaries](https://reference.groupdocs.com/search/net/groupdocs.search/index/dictionaries/) object. Like the [alphabet]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}), the tokenization dictionary is an instance of the [Alphabet](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/) class and contains the types of characters, but it is used for another purpose.

## Purpose of the tokenization dictionary

By default, the text of documents is divided into words by the built-in mechanism, which uses the alphabet: words are formed from continuous sequences of letters, and separators divide them. Some languages, in particular Chinese, Japanese and Korean, do not use spaces between words and require special text segmentation tools. For such documents, you can use an external text segmenter: implement the [IWordSplitter](https://reference.groupdocs.com/search/net/groupdocs.search.common/iwordsplitter/) interface and assign an instance to the [WordSplitter](https://reference.groupdocs.com/search/net/groupdocs.search.events/fileindexingeventargs/wordsplitter/) property of the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event arguments. For details, see the [Custom text segmenter]({{< ref "search/net/developer-guide/advanced-usage/indexing/custom-text-segmenter.md" >}}) page.

When a document is indexed with a custom word splitter, the alphabet is not used to divide its text into words. The word splitter determines the boundaries of words, and the tokenization dictionary determines which of the returned words are indexed:

*   a word is indexed if at least one of its characters has a type other than [CharacterType](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/charactertype/).Separator in the tokenization dictionary;
*   a word that consists only of separators, for example, a sequence of spaces or punctuation marks returned by the word splitter, is not indexed.

As in the case of the built-in mechanism, words longer than 80 characters are not indexed, and stop words are not indexed if the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) setting is enabled.

## Default content

By default, the tokenization dictionary defines the following characters as letters (Unicode numbers are given):

*   all characters that are letters in the default alphabet: digits, the low line character, and Latin, Greek and Coptic, Cyrillic, Armenian and Arabic letters (see the [Character types]({{< ref "search/net/developer-guide/advanced-usage/indexing/character-types.md" >}}) page);
*   CJK Radicals Supplement: 2E80-2EFF;
*   CJK Symbols and Punctuation: 3000-303F;
*   CJK Strokes: 31C0-31EF;
*   CJK Compatibility: 3300-33FF;
*   CJK Unified Ideographs Extension A: 3400-4DBF;
*   CJK Unified Ideographs: 4E00-9FFF;
*   CJK Compatibility Ideographs: F900-FAFF;
*   CJK Compatibility Forms: FE30-FE4F;
*   CJK Unified Ideographs Extensions B–H: 20000-2A6DF, 2A700-2B73F, 2B740-2B81F, 2B820-2CEAF, 2CEB0-2EBEF, 30000-3134F, 31350-323AF;
*   CJK Compatibility Ideographs Supplement: 2F800-2FA1F.

All other characters, including Japanese Hiragana and Katakana and Korean Hangul, are separators in the default tokenization dictionary. If a word splitter returns words of such characters, add the characters to the tokenization dictionary, as shown in the example below.

## Managing the tokenization dictionary

The tokenization dictionary provides the same methods as the alphabet:

*   The [GetCharacterType](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/getcharactertype/) method returns the type of a character specified as a *char* value, as a Unicode code point, or as a string that contains a character or a surrogate pair.
*   The [Count](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/count/) property returns the number of characters of a type other than [CharacterType](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/charactertype/).Separator; enumerating the dictionary returns the Unicode code points of these characters.
*   The [SetRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/setrange/) method sets the type of characters specified as *char* values, as Unicode code points, or as strings that contain characters or surrogate pairs.
*   The [Clear](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/alphabet/clear/) method sets the [CharacterType](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/charactertype/).Separator type for all characters. After that, no word returned by a custom word splitter is indexed until letters are defined again.
*   The [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) and [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) methods save the dictionary to a file and load it from a file. The imported types are set for the characters listed in the file; the types of other characters are not changed. Errors of exporting and importing are reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index instead of exceptions.

{{< alert style="info" >}}The tokenization dictionary defines which terms of documents are indexed. Therefore, the tokenization dictionary of an index can be changed only while the index contains no documents and is not performing an operation; otherwise, a method that changes the dictionary throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception). A call that does not change the content of the dictionary is allowed. Configure the tokenization dictionary after creating a new index and before adding documents. See also the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

In a search network, the tokenization dictionary of a shard is obtained with the [GetTokenizationDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/gettokenizationdictionary/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class; see the [Managing dictionaries in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/managing-dictionaries-in-network.md" >}}) page.

## Example

The following example demonstrates a simple word splitter that divides text into words at white space characters. The words it returns may consist only of punctuation marks, for example, a standalone dash; such words are not indexed, because the tokenization dictionary defines all their characters as separators.

**C#**

```csharp
// A simple word splitter that divides text into words at white space characters
public class WhiteSpaceWordSplitter : IWordSplitter
{
    public IEnumerable<string> Split(string text)
    {
        return text.Split((char[])null, StringSplitOptions.RemoveEmptyEntries);
    }
}
```

The following example demonstrates how to configure the tokenization dictionary of a new index and use the word splitter from the previous example to index text documents.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating a new index in the specified folder;
// the tokenization dictionary can be changed only while the index contains no documents
Index index = new Index(indexFolder, true);

Alphabet tokenizationDictionary = index.Dictionaries.TokenizationDictionary;

// Getting the type of a Hiragana character, which is a separator by default
Console.WriteLine("The type of U+3042: " + tokenizationDictionary.GetCharacterType('\u3042'));

// Setting the Letter type for Hiragana and Katakana characters
List<int> codePoints = new List<int>();
for (int codePoint = 0x3040; codePoint <= 0x30FF; codePoint++)
{
    codePoints.Add(codePoint);
}
tokenizationDictionary.SetRange(codePoints.ToArray(), CharacterType.Letter);

// Getting the number of characters that are not separators
Console.WriteLine("Characters that are not separators: " + tokenizationDictionary.Count);

// Exporting the tokenization dictionary to a file, for example, to use it in other indexes
tokenizationDictionary.ExportDictionary(@"c:\TokenizationDictionary.dat");

// Using the custom word splitter for text documents
WhiteSpaceWordSplitter wordSplitter = new WhiteSpaceWordSplitter();
index.Events.FileIndexing += (sender, args) =>
{
    if (args.DocumentFullPath.EndsWith(".txt", StringComparison.OrdinalIgnoreCase))
    {
        args.WordSplitter = wordSplitter;
    }
};

// Indexing documents; after that, the tokenization dictionary of the index cannot be changed
index.Add(documentsFolder);
```

The following example demonstrates how to create another index with the same tokenization dictionary by importing the dictionary exported in the previous example.

**C#**

```csharp
string newIndexFolder = @"c:\MyNewIndex\";

// Creating a new index in the specified folder
Index newIndex = new Index(newIndexFolder, true);

// Setting the Separator type for all characters, so that only the imported types remain in the dictionary
newIndex.Dictionaries.TokenizationDictionary.Clear();

// Importing the tokenization dictionary from a file
newIndex.Dictionaries.TokenizationDictionary.ImportDictionary(@"c:\TokenizationDictionary.dat");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
