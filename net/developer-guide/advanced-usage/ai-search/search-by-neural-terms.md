---
id: search-by-neural-terms
url: search/net/search-by-neural-terms
title: Search by neural terms
weight: 4
description: "This article describes the search by neural terms in GroupDocs.Search for .NET: a neural sparse model expands every passage of a document with the words that it is about, so the ranked search finds documents by meaning without running a model at search time."
keywords: neural terms, neural sparse search, learned sparse retrieval, document expansion, semantic search, opensearch neural sparse, matched tokens
productName: GroupDocs.Search for .NET
hideChildren: False
---
The search by words finds a document only if it has the words of the query. The search by neural terms finds a document that is about the query, even if it uses another form of a word or another word for the same thing.

A neural terms model reads every passage of a document once and writes to the index the words that the passage is about, each with a weight. Among them are the words of the passage itself and words that the passage does not have: for a passage about the termination of a lease, the model may add "rent", "tenant" and "end". These words with their weights are the neural terms of the passage.

A query is encoded without a neural network: every token of the query gets its weight from a table of the model, which is kept in the index. A passage is scored by the tokens that it shares with the query. Therefore:

*   The model and the runtime are needed only to [encode the documents]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md" >}}). An encoded index is searched without them, on any computer.
*   A search is fast, because no network is run for a query.
*   The result explains itself: it shows the tokens by which a passage is found.

The neural terms are the [NeuralTerms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankingchannels/) channel of the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}). Among the channels, it gives the best quality when the query and the documents are in the same language, see [Quality and performance of AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-quality.md" >}}).

## Encoding and searching

You need a neural terms [model package]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md" >}}), the GroupDocs.Search.AI.Onnx package and ONNX Runtime. The index must [store the text]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}) of its documents.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string modelFolder = @"c:\Models\opensearch-neural-sparse-encoding-multilingual-v1-int8";

// Creating an index that stores the text of the documents
IndexSettings settings = new IndexSettings();
settings.TextStorageSettings = new TextStorageSettings(Compression.High);
Index index = new Index(indexFolder, settings);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Once: the model encodes the stored text of the documents
index.AI.Runtime = new OnnxModelRuntime();
index.AI.AddModel(ModelPackage.Load(modelFolder));
index.AI.Encode();

// Every search: neither the model nor the runtime is used
RankedSearchResult result = index.AI.Search("how to end a rental agreement early");
foreach (RankedDocument document in result)
{
    Console.WriteLine(document.DocumentInfo.FilePath);
}
```

An application that only searches opens the encoded index and searches. It needs neither the model package nor the GroupDocs.Search.AI.Onnx package.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an encoded index
Index index = new Index(indexFolder);

// The search by the text and by the neural terms
RankedSearchResult result = index.AI.Search("how to end a rental agreement early");
```

## Why a passage is found

For every found passage, the result gives the tokens of the query that the passage has among its neural terms. The [MatchedTokenCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/matchedtokencount/) property and the [GetMatchedToken](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/getmatchedtoken/) method of the [FoundPassage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/) class return them as instances of the [MatchedToken](https://reference.groupdocs.com/search/net/groupdocs.search.ai/matchedtoken/) class, ordered by their shares in the score of the passage:

| Property | Description |
| --- | --- |
| [Token](https://reference.groupdocs.com/search/net/groupdocs.search.ai/matchedtoken/token/) | The token as the vocabulary of the model has it. A token that continues a word has the prefix of the model, usually "##". |
| [Weight](https://reference.groupdocs.com/search/net/groupdocs.search.ai/matchedtoken/weight/) | The share of the token in the score of the passage: the weight of the token in the query multiplied by its weight in the passage. |
| [IsInText](https://reference.groupdocs.com/search/net/groupdocs.search.ai/matchedtoken/isintext/) | Whether the text of the passage has the token. A token that the text does not have is given to the passage by the model: the passage is about it without naming it. |

The following example searches by the neural terms only and prints the tokens by which the best passage is found.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an encoded index
Index index = new Index(indexFolder);

RankedSearchOptions options = new RankedSearchOptions();
options.Channels = RankingChannels.NeuralTerms;
RankedSearchResult result = index.AI.Search("how to end a rental agreement early", options);

if (result.DocumentCount > 0 && result.GetFoundDocument(0).PassageCount > 0)
{
    FoundPassage passage = result.GetFoundDocument(0).GetPassage(0);
    Console.WriteLine(passage.Text);
    for (int i = 0; i < passage.MatchedTokenCount; i++)
    {
        MatchedToken token = passage.GetMatchedToken(i);
        string origin = token.IsInText ? "in the text" : "added by the model";
        Console.WriteLine(token.Token + "  " + token.Weight.ToString("F2") + "  " + origin);
    }
}
```

## What to expect

*   **Quality.** On a test set of questions and paragraphs in one language, the text channel together with the neural terms has found the right paragraph first for 91 to 95 percent of the questions, against 80 to 92 percent of the text channel alone. The gain is the greatest for the languages with many forms of a word, such as Russian and German.
*   **Languages.** The tested model is multilingual. It matches the words of a query and of a document mostly within one language and one script. To find documents by a query in another language, use the [passage vectors]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-passage-vectors.md" >}}); the two channels work together.
*   **Time of the encoding.** The encoding by a neural network is the longest operation of the AI search, see [Encoding documents]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md#speed-and-size" >}}). It is done once for a document, it can be cancelled and continued, and it does not block the searches.
*   **Size.** The neural terms take about 0.55 KB of the index for a passage.
*   **Exact conditions.** The neural terms order the documents and do not replace the exact search. To require a word, a phrase or a date range, set the [Filter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/filter/) option of the ranked search or use the usual search.

## Notes

*   An index remembers the model that has encoded its documents and accepts only that model afterwards, see [Changing the model]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md#changing-the-model" >}}).
*   The model package has two variants of the network, with full and with 8-bit weights, see [Models for AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md#neural-terms-model-with-8-bit-weights" >}}).
*   The documents that are added after the encoding are found by the text channel until the [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method is called again.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
