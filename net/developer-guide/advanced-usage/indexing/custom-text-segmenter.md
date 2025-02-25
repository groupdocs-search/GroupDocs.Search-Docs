---
id: custom-text-segmenter
url: search/net/custom-text-segmenter
title: Custom text segmenter
weight: 3
description: "GroupDocs.Search provides the ability to set up custom text segmenting."
keywords: custom text segmenter, word splitter, text splitter
productName: GroupDocs.Search for .NET
hideChildren: False
---

Some languages, in particular hieroglyphic languages, typically require special tools to split text into words.
This is necessary due to the lack of spaces between words, polysemantic phrases, set expressions, and complex lexical rules.

Currently, the GroupDocs.Search library does not contain such special algorithms for splitting Chinese, Japanese, and Korean text into words. However, it provides the ability to use external custom algorithms for this. To connect an external text segmentation library, you must implement the [IWordSplitter](https://reference.groupdocs.com/search/net/groupdocs.search.common/iwordsplitter/) interface and pass an object implementing this interface to the [WordSplitter](https://reference.groupdocs.com/search/net/groupdocs.search.events/fileindexingeventargs/wordsplitter/) property of the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event arguments.

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
