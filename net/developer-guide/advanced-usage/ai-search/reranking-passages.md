---
id: reranking-passages
url: search/net/reranking-passages
title: Reranking of passages
weight: 6
description: "This article describes the reranking of passages in GroupDocs.Search for .NET: a cross-encoder model reads the query together with each of the best passages of the ranked search and orders the found documents more exactly."
keywords: reranking, reranker, cross-encoder, NeuralReranker, IReranker, UseReranker, ranked search
productName: GroupDocs.Search for .NET
hideChildren: False
---
The channels of the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}) compare a query with data that is prepared in advance: words, neural terms and vectors. A reranker is a model that reads the text of the query and the text of a passage together and returns how well the passage answers the query. This is more exact, and much slower: the network is run for every passage. Therefore, a reranker does not search the index. It orders again only the best passages that the channels have found.

## How the reranking works

A search uses the reranker when the [UseReranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/usereranker/) option is set and the index has a reranker in the [Reranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/reranker/) property:

1.  The channels find the documents and their passages, and the lists of the channels are merged, as in any ranked search.
2.  The reranker scores the passages of the first documents of the merged list, at most [RerankedPassageCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/rerankedpassagecount/) passages: 50 by default.
3.  The documents whose passages are scored are ordered by the scores of their best passages, within the places that these documents occupied. A document that has no passages stays at its place.
4.  The scored passages of a document go first, in the order of their scores.

In the result:

*   The [Reranked](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchresult/reranked/) property tells whether the reranker has ordered the documents. If the reranker is not set, the search is performed without it.
*   The [IsReranked](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/isreranked/) and [RerankerScore](https://reference.groupdocs.com/search/net/groupdocs.search.ai/foundpassage/rerankerscore/) properties of a passage give the score of the reranker. The scale of the scores is the one of the model.
*   The [Score](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankeddocument/score/) property of a document stays as the channels have made it.

The reranker scores the passages that the search has, so the documents need passages. The neural terms and the passage vectors give them for the encoded documents. In an index whose documents are not encoded, set the [TextPassageDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/textpassagedocumentcount/) option: the passages of the text channel are reranked too.

## Neural reranker

The library has a reranker of its own, the [NeuralReranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/neuralreranker/) class: a neural network of a [model package]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md" >}}) of the [Reranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelrole/) role. The library divides the texts into tokens, and a runtime runs the network, see [ONNX Runtime adapter]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md#onnx-runtime-adapter" >}}).

The [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method creates the object for such a package and sets it to the [Reranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/reranker/) property. The following example reranks the passages in an index whose documents are not encoded.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\mmarco-mMiniLMv2-L12-H384-v1";

// Opening an index that stores the text of its documents
Index index = new Index(indexFolder);

// The runtime runs the network of the reranker
index.AI.Runtime = new OnnxModelRuntime();
index.AI.AddModel(ModelPackage.Load(modelFolder));

RankedSearchOptions options = new RankedSearchOptions();
options.UseReranker = true;
options.RerankedPassageCount = 20; // The reranker scores at most 20 passages
options.TextPassageDocumentCount = 20; // The first 20 documents get passages from the text channel

RankedSearchResult result = index.AI.Search("how to end a rental agreement early", options);
Console.WriteLine("Reranked: " + result.Reranked);
foreach (RankedDocument document in result)
{
    Console.WriteLine(document.DocumentInfo.FilePath);
    if (document.PassageCount > 0 && document.GetPassage(0).IsReranked)
    {
        Console.WriteLine("    " + document.GetPassage(0).RerankerScore.ToString("F2"));
    }
}
```

The object that the [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method creates belongs to the index. To use one network in several indexes, create a [NeuralReranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/neuralreranker/) object yourself, set it to the property of every index and dispose it when the indexes are closed.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\mmarco-mMiniLMv2-L12-H384-v1";

// Opening an index
Index index = new Index(indexFolder);

ModelPackage package = ModelPackage.Load(modelFolder);
using (NeuralReranker reranker = new NeuralReranker(package, new OnnxModelRuntime()))
{
    index.AI.Reranker = reranker;

    RankedSearchOptions options = new RankedSearchOptions();
    options.UseReranker = true;
    RankedSearchResult result = index.AI.Search("how to end a rental agreement early", options);
}
```

A query and a passage that together are longer than the limit of the network, usually 512 tokens, are shortened from the end of the longer text, so a short query stays whole.

## Reranker of your application or of a service

Any reranker can be connected by an implementation of the [IReranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ireranker/) interface. Its only method, [Score](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ireranker/score/), takes the text of a query and the texts of passages and returns the scores of the passages in their order: the greater the score, the more relevant the passage. The method can be called by several threads at the same time.

**C#**

```csharp
public class MyReranker : IReranker
{
    public float[] Score(string query, string[] passages)
    {
        float[] scores = new float[passages.Length];
        for (int i = 0; i < passages.Length; i++)
        {
            scores[i] = GetScore(query, passages[i]);
        }

        return scores;
    }

    private float GetScore(string query, string passage)
    {
        // Call your model or service here
        throw new NotImplementedException();
    }
}
```

If the reranker is a remote service, the text of the query and the texts of the best passages leave the computer.

## What to expect

*   The network is run for every reranked passage, so the time of a search grows in proportion to the number of the reranked passages. Choose the [RerankedPassageCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchoptions/rerankedpassagecount/) option by the time that a search may take on your hardware.
*   The reranker changes the order of the documents that the channels have found. It cannot find a document that the channels have missed, so it complements the neural terms and the vectors and does not replace them.
*   Measure the effect on your own documents and queries: it depends on the model and on the language.

## Notes

*   An exception of the reranker is thrown by the [Search](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/search/) method. The method throws [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception) if the reranker has returned a result that is not valid.
*   The [UseReranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/usereranker/) option of the [answers with citations]({{< ref "search/net/developer-guide/advanced-usage/ai-search/answers-with-citations.md" >}}) orders the passages by the reranker before the best of them are given to the language model.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
