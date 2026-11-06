---
id: custom-text-segmenter
url: search/net/custom-text-segmenter
title: Custom text segmenter
weight: 5
description: "This article explains how to use an external library to split text into words during indexing by implementing the IWordSplitter interface."
keywords: custom text segmenter, word splitter, text splitter
productName: GroupDocs.Search for .NET
hideChildren: False
---

Some languages, in particular hieroglyphic languages, typically require special tools to split text into words.
This is necessary due to the lack of spaces between words, polysemantic phrases, set expressions, and complex lexical rules.

Currently, the GroupDocs.Search library does not contain such special algorithms for splitting Chinese, Japanese, and Korean text into words. However, it provides the ability to use external custom algorithms for this. To connect an external text segmentation library, you must implement the [IWordSplitter](https://reference.groupdocs.com/search/net/groupdocs.search.common/iwordsplitter/) interface and pass an object implementing this interface to the [WordSplitter](https://reference.groupdocs.com/search/net/groupdocs.search.events/fileindexingeventargs/wordsplitter/) property of the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event arguments.

The words returned by the word splitter are checked against the [tokenization dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary.md" >}}) of the index ([Dictionaries.TokenizationDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/tokenizationdictionary/)): a word is indexed only if it contains at least one character that is not a separator in this dictionary; other words, for example punctuation marks, are skipped. The tokenization dictionary has the same structure as the alphabet, and by default it also defines the CJK ideographs as letters. Like the alphabet, it can be changed only while the index contains no documents. Since version 26.11, the default alphabet and tokenization dictionary of a new index also define the Hangul characters as letters, so the Korean words of a splitter are indexed; an index created by an earlier version keeps its dictionaries (see [Character types]({{< ref "search/net/developer-guide/advanced-usage/indexing/character-types.md" >}})). Note also that words longer than 80 characters are not indexed.

Since version 26.11:

*   The words that the splitter gives get the [character replacements]({{< ref "search/net/developer-guide/advanced-usage/indexing/character-replacement-during-indexing.md" >}}) of the index, if it uses them, as the other words of the documents do. The documents that an earlier version indexed with a custom word splitter keep their words without the replacements, so a word with a replaced character does not find them: index such documents again.
*   A text that fails while it is read is split up to the failure: the words read before the failure are indexed, the failure is reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event, and the document is marked with the [IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/) flag, so the next [update]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md" >}}) indexes its file again. Before, the text ended without an error.
*   An index that [stores the texts of the documents]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}) stores the whole text of a document indexed with a splitter: the words of the splitter are looked for in the text one after another, and the text between them, for example the spaces and the punctuation, is stored as well. A word that the splitter has changed, for example to its base form, is indexed at its position, but the stored text keeps the text of the document. Before, only the words of the splitter were stored: a document indexed so by an earlier version gets its whole text when it is indexed again.
*   The highlighting of a document indexed with a splitter needs the same splitter: assign it to the [WordSplitter](https://reference.groupdocs.com/search/net/groupdocs.search.options/textoptions/wordsplitter/) property of the highlight options, so that the words of the splitter are highlighted. See [Highlighting search results]({{< ref "search/net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}).

The code example below demonstrates how to use the external library [Jieba.NET](https://www.nuget.org/packages/jieba.NET/) for Chinese text segmentation.

**C#**

```csharp
// Implementing custom word splitter
public class JiebaWordSplitter : IWordSplitter
{
    private readonly JiebaSegmenter segmenter;

    public JiebaWordSplitter()
    {
        segmenter = new JiebaSegmenter();
    }

    public IEnumerable<string> Split(string text)
    {
        IEnumerable<string> segments = segmenter.Cut(text, cutAll: false);
        return segments;
    }
}

...

string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Using Jieba segmenter to break text into words
JiebaWordSplitter jiebaWordSplitter = new JiebaWordSplitter();
index.Events.FileIndexing += (s, e) =>
{
    if (e.DocumentFullPath.EndsWith("Chinese.txt"))
    {
        // We know that the text in this document is in Chinese
        e.WordSplitter = jiebaWordSplitter;
    }
};

// Indexing documents
index.Add(documentsFolder);

// Searching in the index
string query = "考虑"; // Consider
SearchResult result = index.Search(query);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
