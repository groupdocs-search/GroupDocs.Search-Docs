---
id: ranked-search
url: search/net/ranked-search
title: Ranked search
weight: 1
description: "This article describes the ranked search of GroupDocs.Search for .NET: a search by a query in a natural language that orders the documents by relevance, merges several ranking channels and returns the best passages of the text."
keywords: ranked search, relevance, natural language query, hybrid search, reciprocal rank fusion, BM25, passages, ranking channels
productName: GroupDocs.Search for .NET
hideChildren: False
---
The ranked search finds the documents that are relevant to a query in a natural language and orders them by the relevance, the most relevant first. It is performed by the [Search](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/search/) method of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class, which is given by the [AI](https://reference.groupdocs.com/search/net/groupdocs.search/index/ai/) property of an index.

The ranked search differs from the usual search of the [Index.Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method:

| | Usual search | Ranked search |
| --- | --- | --- |
| Query | A query of the [query language]({{< ref "search/net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}), or a query in object form. Several words have to be joined by operators or put into a phrase. | A question, a phrase or words. The text is not parsed: any text is a valid query, and the characters of the operators are plain characters. |
| Found documents | All the documents that match the query exactly. | The documents that are the most relevant to the query; a document does not have to contain all the words of the query, and with the models it may contain none of them. |
| Order | The order of the index, or the relevance, see [Search options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md#resultorder-property" >}}). | Always the relevance. |
| Size of the result | All the found documents. | The first documents: 10 by default. |
| Details of a document | The found terms and phrases with their occurrences. | The best passages of the text, with the explanation of why they are found. |

The ranked search works in any index and needs no model. The models add the search by meaning to it, see [Channels](#channels).

## Performing a search

The following example searches by a question and prints the found documents.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Searching by a query in a natural language
RankedSearchResult result = index.AI.Search("how to end a rental agreement early");

// Printing the found documents, the most relevant first
Console.WriteLine("Documents: " + result.DocumentCount);
foreach (RankedDocument document in result)
{
    Console.WriteLine(document.Score.ToString("F4") + "  " + document.DocumentInfo.FilePath);
}
```

The [Search](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/search/) method has an overload that takes the options of the search, an instance of the [RankedSearchOptions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/) class.

## Channels

A channel is a way to order the documents by a query. The ranked search has three channels, the values of the [RankingChannels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankingchannels/) enumeration:

| Channel | How it finds | What it needs |
| --- | --- | --- |
| [Text](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankingchannels/) | By the words of the query in the text of the documents. The first 64 words of the query, not counting the stop words of an index that uses them, are joined by OR, and the documents are ordered by the Okapi BM25 score. | Nothing. It works in any index. |
| [NeuralTerms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankingchannels/) | By the neural terms of the passages: the words that a model has found a passage to be about, see [Search by neural terms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-neural-terms.md" >}}). | The documents [encoded]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md" >}}) by a neural terms model. The search itself needs neither the model nor a runtime. |
| [PassageVectors](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankingchannels/) | By the direction of the vectors of the passages relative to the vector of the query, see [Search by passage vectors]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-passage-vectors.md" >}}). | The documents encoded by a model of vectors, and the same model set by the [Embedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/embedder/) property: it converts every query into a vector. |

By default, a search uses all the channels that the index has data for. A channel that has no data or no model is passed over: for example, an index whose documents are not encoded is searched by the text channel only. The [UsedChannels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchresult/usedchannels/) property of the result returns the channels that the search has used. The [Channels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/channels/) property of the options limits the channels of a search.

A document that is added to an encoded index is found by the text channel at once and by the other channels after the next encoding.

### How the channels are merged

Each channel makes its own list of documents, and the lists are merged by the weighted reciprocal rank fusion. A channel that has found a document gives it

`weight / (60 + place)`

where the place is the place of the document in the list of the channel, starting from 1, and the weight, from 0 to 1, tells how well the document matches the query in the channel. The score of a document is the sum over the channels, so a document that several channels have found near the top goes first.

The weight of the first document of a channel is:

*   in the text channel, the share of the words of the query that the document has; a rare word counts for more than a frequent one;
*   in the neural terms channel, the share of the tokens of the query that the best passage of the document has, by the weights of the tokens;
*   in the passage vectors channel, the cosine of the angle between the vector of the query and the vector of the best passage of the document.

The weight of another document of a channel is the weight of the first one lowered in proportion to the score of the document in the channel. Therefore, a search by one channel gives the order of that channel, and a channel that has found little of what is asked does not outweigh a channel that has found it. For example, when a query is written in a language that the documents are not written in, the channels of the words find only the documents that happen to have a name or a number of the query, with small weights, and the documents that the passage vectors channel finds by the meaning go first.

The score of a document is comparable only within one result: it is not a probability and it cannot be compared with a fixed threshold.

## Options of the search

| Property | Description | Default |
| --- | --- | --- |
| [MaxResultCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/maxresultcount/) | The greatest number of the documents of the result, from 1 to 100,000. | 10 |
| [Channels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/channels/) | The channels that the search uses. | All |
| [Filter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/filter/) | The query that limits the documents of the search. | null |
| [SearchDocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/searchdocumentfilter/) | The filter of the documents by their paths and attributes. | null |
| [MaxPassagesPerDocument](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/maxpassagesperdocument/) | The greatest number of the passages that the result gives for a document, from 1 to 100. | 1 |
| [TextPassageDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/textpassagedocumentcount/) | The number of the first documents of the result that get passages from the text channel when the other channels have found no passages of them, from 0 to 1000. | 0 |
| [UseReranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/usereranker/) | Whether the best passages are scored by the reranker of the index, see [Reranking of passages]({{< ref "search/net/developer-guide/advanced-usage/ai-search/reranking-passages.md" >}}). | false |
| [RerankedPassageCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/rerankedpassagecount/) | The greatest number of the passages that the reranker scores, from 1 to 1000. | 50 |
| [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/cancellation/) | The object for cancelling the search. | null |

## Limiting the documents of a search

The text of a query of the ranked search has no operators. The exact conditions are given separately, and they limit the documents that are ranked without changing their order:

*   The [Filter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/filter/) property takes a [query in object form]({{< ref "search/net/developer-guide/advanced-usage/searching/queries-in-text-and-object-form.md" >}}). Only the documents that the query finds are ranked. The query is searched as the usual search searches it, so every exact condition works: a phrase, a field, a range of dates or numbers, a negation.
*   The [SearchDocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/searchdocumentfilter/) property takes a filter of the documents by their paths, extensions and [attributes]({{< ref "search/net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}), as the [filter of the usual search]({{< ref "search/net/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}).

The following example ranks only the PDF documents that have the word "lease" and a date of the year 2020 in their text, see [Date range search]({{< ref "search/net/developer-guide/advanced-usage/searching/date-range-search.md" >}}).

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);

// The exact conditions: the word "lease" and a date of 2020 in the text of a document
SearchQuery word = SearchQuery.CreateWordQuery("lease");
SearchQuery dates = SearchQuery.CreateDateRangeQuery(new DateTime(2020, 1, 1), new DateTime(2020, 12, 31));

RankedSearchOptions options = new RankedSearchOptions();
options.Filter = SearchQuery.CreateAndQuery(word, dates);
options.SearchDocumentFilter = SearchDocumentFilter.CreateFileExtension(".pdf");

// Only the documents that the filters accept are ranked
RankedSearchResult result = index.AI.Search("early termination by the tenant", options);
```

## Passages

A passage is a part of the text of a field of a document, about a thousand characters long. The result gives the best passages of each found document: the [PassageCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankeddocument/passagecount/) property and the [GetPassage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankeddocument/getpassage/) method of the [RankedDocument](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankeddocument/) class. The passages of a document are ordered by their scores, the best first, and their number is limited by the [MaxPassagesPerDocument](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/maxpassagesperdocument/) option.

The passages are found by the neural terms channel and the passage vectors channel, which compare the query with every passage of the encoded documents. The text channel finds a document by the words of the query in all its text and gives no passages by itself. To get passages in an index whose documents are not encoded, and for the documents that wait for the encoding, set the [TextPassageDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/textpassagedocumentcount/) option: the stored text of the first documents of the result that have no passages is divided into passages, and the passages that have the most of the words of the query are given. The text of every such document is read from the index, which takes time in proportion to the length of the text.

The passages need the [text of the documents stored in the index]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}). The text of a passage is given as the index stores it.

The [FoundPassage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/) class describes a passage:

| Member | Description |
| --- | --- |
| [Text](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/text/) | The text of the passage; an empty string if the text cannot be restored, for example because the stored text of the document is damaged. |
| [FieldName](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/fieldname/) | The name of the field of the document that has the passage. |
| [Score](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/score/) | The score of the passage in the result, made of the channels as the score of a document. |
| [Channels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/channels/) | The channels that have found the passage. |
| [GetScore](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/getscore/) | The score that a channel has given to the passage. The scores of the channels are different measures: the sum of the shares of the matched tokens for the neural terms, the cosine for the vectors, the Okapi BM25 score of the words of the query in the passage for the text channel. |
| [GetWeight](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/getweight/) | The weight of the passage in a channel, from 0 to 1. |
| [MatchedTokenCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/matchedtokencount/), [GetMatchedToken](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/getmatchedtoken/) | The tokens of the query that the passage has among its neural terms, see [Search by neural terms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-neural-terms.md#why-a-passage-is-found" >}}). |
| [IsReranked](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/isreranked/), [RerankerScore](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/rerankerscore/) | Whether the passage is scored by the reranker, and its score. |

## Search results

The [RankedSearchResult](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchresult/) class represents the result of a search:

*   The [DocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchresult/documentcount/) property returns the number of the documents of the result, and the [GetFoundDocument](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchresult/getfounddocument/) method returns a document by its index. The result can also be enumerated.
*   The [UsedChannels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchresult/usedchannels/) property returns the channels that the search has used.
*   The [Reranked](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchresult/reranked/) property tells whether the reranker has ordered the documents.
*   The [Truncated](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchresult/truncated/) property tells whether the result is truncated by the [limitations of the evaluation version]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md#ai-search-limitations" >}}).

The [RankedDocument](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankeddocument/) class represents a found document:

*   The [DocumentInfo](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankeddocument/documentinfo/) property returns the path and the other information of the document.
*   The [Score](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankeddocument/score/) property returns the score by which the documents of the result are ordered.
*   The [GetRank](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankeddocument/getrank/) method returns the place of the document in the list of a channel, starting from 1, or 0 if the channel has not found the document.
*   The [GetWeight](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankeddocument/getweight/) method returns the weight of the document in a channel, from 0 to 1.

The following example prints how each channel has ranked the found documents, and their passages.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);

RankedSearchOptions options = new RankedSearchOptions();
options.MaxResultCount = 5;
options.MaxPassagesPerDocument = 2;
options.TextPassageDocumentCount = 5;

RankedSearchResult result = index.AI.Search("how to end a rental agreement early", options);
Console.WriteLine("Used channels: " + result.UsedChannels);

RankingChannels[] channels = new RankingChannels[]
{
    RankingChannels.Text,
    RankingChannels.NeuralTerms,
    RankingChannels.PassageVectors,
};
for (int i = 0; i < result.DocumentCount; i++)
{
    RankedDocument document = result.GetFoundDocument(i);
    Console.WriteLine(document.DocumentInfo.FilePath + ", score " + document.Score.ToString("F4"));
    foreach (RankingChannels channel in channels)
    {
        int rank = document.GetRank(channel);
        if (rank > 0)
        {
            string weight = document.GetWeight(channel).ToString("F2");
            Console.WriteLine("    " + channel + ": place " + rank + ", weight " + weight);
        }
    }

    for (int j = 0; j < document.PassageCount; j++)
    {
        FoundPassage passage = document.GetPassage(j);
        Console.WriteLine("    [" + passage.FieldName + ", " + passage.Channels + "] " + passage.Text);
    }
}
```

## Notes

*   Deleted documents are not found.
*   A ranked search can be performed while another operation of the index is running, including the indexing and the encoding of the documents, as a [usual search]({{< ref "search/net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}}).
*   An exception of a model of vectors or of a reranker is thrown by the [Search](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/search/) method. The method throws [ArgumentException](https://learn.microsoft.com/dotnet/api/system.argumentexception) if the filter query cannot be searched in the index, and [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception) if a model has returned a result that is not valid.
*   The usual search has options that the ranked search does not use: for example, the [semantic expansion]({{< ref "search/net/developer-guide/advanced-usage/ai-search/semantic-query-expansion.md" >}}), the fuzzy search and the synonyms work in the [Index.Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method. In the ranked search, the same job is done by the neural terms and the passage vectors.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
