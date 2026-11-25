---
id: search-by-passage-vectors
url: search/net/search-by-passage-vectors
title: Search by passage vectors
weight: 5
description: "This article describes the search by passage vectors in GroupDocs.Search for .NET: a model converts every passage and every query into a vector of meaning, so the ranked search finds documents by meaning and between languages."
keywords: vector search, embeddings, semantic search, cross-language search, WordVectorEmbedder, NeuralTextEmbedder, ITextEmbedder, static embeddings, Microsoft.Extensions.AI
productName: GroupDocs.Search for .NET
hideChildren: False
---
A model of vectors converts a text into a vector: a row of numbers that describes the meaning of the text. Texts of close meaning get vectors of close directions, whatever words they are written in and, with a multilingual model, in whatever language.

When the documents are [encoded]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md" >}}), every passage of a document gets its vector. A query of the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}) is converted into a vector by the same model, and the passages are ordered by the cosine of the angle between their vectors and the vector of the query. This is the [PassageVectors](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankingchannels/) channel of the ranked search.

The channel does what the channels of the words cannot do: it finds a document by a query in another language, and a document that describes the subject of the query in completely different words. Within one language, it works together with the text channel and the neural terms, see [Quality and performance of AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-quality.md" >}}).

Unlike a neural terms model, a model of vectors is needed by every search, because it converts the query. It is set by the [Embedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/embedder/) property of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class and can be:

| Model | Class | What it needs |
| --- | --- | --- |
| A word vectors model | [WordVectorEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/wordvectorembedder/) | A model package of 55 MB. No neural network, no runtime and no service. |
| A neural model of vectors | [NeuralTextEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/neuraltextembedder/) | A model package, the GroupDocs.Search.AI.Onnx package and ONNX Runtime, for the encoding and for the searches. |
| A model of your application or of a service | An implementation of the [ITextEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/itextembedder/) interface | The model or the service. |

## Word vectors model

A word vectors model keeps a vector for every word of its vocabulary; the vector of a text is the mean of the vectors of its words. The model is not a neural network: it encodes a thousand passages or more in a second on one processor core. It is the simplest way to the search by meaning and between languages.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string modelFolder = @"c:\Models\static-similarity-mrl-multilingual-v1";

// Creating an index that stores the text of the documents
IndexSettings settings = new IndexSettings();
settings.TextStorageSettings = new TextStorageSettings(Compression.High);
Index index = new Index(indexFolder, settings);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// The model encodes the passages of the documents
index.AI.Embedder = new WordVectorEmbedder(ModelPackage.Load(modelFolder));
index.AI.Encode();

// A query in Russian finds the documents in English that are about the early termination of a lease
RankedSearchResult result = index.AI.Search("досрочное расторжение договора аренды");
foreach (RankedDocument document in result)
{
    Console.WriteLine(document.DocumentInfo.FilePath);
}
```

When the index is opened again, set the model again before searching: the index keeps only the identifier of the model and the number of the components of a vector.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\static-similarity-mrl-multilingual-v1";

// Opening an encoded index
Index index = new Index(indexFolder);

// The model converts the queries into vectors
index.AI.Embedder = new WordVectorEmbedder(ModelPackage.Load(modelFolder));

RankedSearchResult result = index.AI.Search("how to end a rental agreement early");
Console.WriteLine("Used channels: " + result.UsedChannels);
```

If the model is not set, or is not the model that has encoded the documents of the index, a search passes over the channel: the [UsedChannels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/rankedsearchresult/usedchannels/) property of the result shows it.

The same model package serves the [semantic expansion of queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/semantic-query-expansion.md" >}}) of the usual search.

## Neural model of vectors

A neural model of vectors reads a text as a whole, with the order of its words, so it can describe the meaning of a text more exactly than a mean of word vectors. It is also much slower: a neural network is run for every passage during the encoding and for every query during a search.

The library divides the text into tokens itself, and a runtime runs the network, see [ONNX Runtime adapter]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md#onnx-runtime-adapter" >}}). The simplest way is the [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method: for a model package of the [PassageVectors](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelrole/) role, it creates a [NeuralTextEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/neuraltextembedder/) object and sets it to the [Embedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/embedder/) property. The index releases the object when another model takes its place and when the index is disposed.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\multilingual-e5-small";

// Opening an index that stores the text of its documents
Index index = new Index(indexFolder);

// The runtime runs the network of the model
index.AI.Runtime = new OnnxModelRuntime();
index.AI.AddModel(ModelPackage.Load(modelFolder));

// Encoding the documents and searching
index.AI.Encode();
RankedSearchResult result = index.AI.Search("how to end a rental agreement early");
```

To use one network in several indexes, create the [NeuralTextEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/neuraltextembedder/) object yourself, set it to the property of every index and dispose it when the indexes are closed. The network is loaded once, when it is first needed.

**C#**

```csharp
string modelFolder = @"c:\Models\multilingual-e5-small";

// Two encoded indexes
Index contracts = new Index(@"c:\Indexes\Contracts\");
Index letters = new Index(@"c:\Indexes\Letters\");

// One model object serves both indexes
ModelPackage package = ModelPackage.Load(modelFolder);
using (NeuralTextEmbedder embedder = new NeuralTextEmbedder(package, new OnnxModelRuntime()))
{
    contracts.AI.Embedder = embedder;
    letters.AI.Embedder = embedder;

    RankedSearchResult result1 = contracts.AI.Search("early termination by the tenant");
    RankedSearchResult result2 = letters.AI.Search("early termination by the tenant");
}
```

Notes on the neural models of vectors:

*   A text that is longer than the limit of the network, usually 512 tokens, is read from its beginning. The passages of an index are shorter than the limit.
*   Some models expect a short text before a query and before a document: for example, the E5 models expect "query: " and "passage: ". The model package declares these texts, and the library adds them.
*   The members of the [NeuralTextEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/neuraltextembedder/) class are thread-safe. The network is run by one thread at a time.

## Model of your application or of a service

Any model of vectors can be connected by an implementation of the [ITextEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/itextembedder/) interface. The interface has three members:

| Member | Description |
| --- | --- |
| [ModelId](https://reference.groupdocs.com/search/net/groupdocs.search.ai/itextembedder/modelid/) | The identifier of the model, for example its name with its version: a text of 1 to 256 characters. The index remembers it. |
| [Dimension](https://reference.groupdocs.com/search/net/groupdocs.search.ai/itextembedder/dimension/) | The number of the components of a vector, from 1 to 8192. |
| [Embed](https://reference.groupdocs.com/search/net/groupdocs.search.ai/itextembedder/embed/) | Converts texts into vectors, in the order of the texts. The second argument, a value of the [EmbeddingPurpose](https://reference.groupdocs.com/search/net/groupdocs.search.ai/embeddingpurpose/) enumeration, tells whether the texts are queries or passages of documents: many models convert them differently. The vectors may have any length, because the index compares their directions. |

The members can be called by several threads at the same time: a search converts a query while the documents are encoded.

**C#**

```csharp
public class MyTextEmbedder : ITextEmbedder
{
    public string ModelId
    {
        get { return "my-embedding-model:1"; }
    }

    public int Dimension
    {
        get { return 768; }
    }

    public float[][] Embed(string[] texts, EmbeddingPurpose purpose)
    {
        bool isQuery = purpose == EmbeddingPurpose.Query;
        float[][] vectors = new float[texts.Length][];
        for (int i = 0; i < texts.Length; i++)
        {
            vectors[i] = GetVector(texts[i], isQuery);
        }

        return vectors;
    }

    private float[] GetVector(string text, bool isQuery)
    {
        // Call your model or service here
        throw new NotImplementedException();
    }
}
```

If the model is a remote service, the texts of the passages of the documents and the texts of the queries leave the computer. The library gives them only to the object that the application has set by the [Embedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/embedder/) property.

### An embedding generator of Microsoft.Extensions.AI

The clients of many services implement the `IEmbeddingGenerator<string, Embedding<float>>` interface of [Microsoft.Extensions.AI](https://learn.microsoft.com/dotnet/ai/microsoft-extensions-ai). Such a generator becomes a model of vectors of an index by a small class of your application; the library itself does not depend on that package.

The identifier of the model and the number of the components of a vector are given explicitly, because the index remembers them and they must not change with the settings of a client.

**C#**

```csharp
public class EmbeddingGeneratorTextEmbedder : ITextEmbedder
{
    private readonly IEmbeddingGenerator<string, Embedding<float>> generator;
    private readonly string modelId;
    private readonly int dimension;

    public EmbeddingGeneratorTextEmbedder(
        IEmbeddingGenerator<string, Embedding<float>> generator,
        string modelId,
        int dimension)
    {
        this.generator = generator;
        this.modelId = modelId;
        this.dimension = dimension;
    }

    public string ModelId
    {
        get { return modelId; }
    }

    public int Dimension
    {
        get { return dimension; }
    }

    public float[][] Embed(string[] texts, EmbeddingPurpose purpose)
    {
        // The texts that the model expects before a query and before a document, if it expects any
        string prefix = purpose == EmbeddingPurpose.Query ? "search_query: " : "search_document: ";
        string[] inputs = new string[texts.Length];
        for (int i = 0; i < texts.Length; i++)
        {
            inputs[i] = prefix + texts[i];
        }

        // A generator is asynchronous: it is called by a thread of the thread pool, and this thread waits
        GeneratedEmbeddings<Embedding<float>> embeddings =
            Task.Run(() => generator.GenerateAsync(inputs)).GetAwaiter().GetResult();
        float[][] vectors = new float[texts.Length][];
        for (int i = 0; i < texts.Length; i++)
        {
            vectors[i] = embeddings[i].Vector.ToArray();
        }

        return vectors;
    }
}
```

The class is used as any other model of vectors:

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index that stores the text of its documents
Index index = new Index(indexFolder);

// An embedding generator that the application creates by the package of its service
IEmbeddingGenerator<string, Embedding<float>> generator = CreateEmbeddingGenerator();

index.AI.Embedder = new EmbeddingGeneratorTextEmbedder(generator, "nomic-embed-text-v1.5", 768);
index.AI.Encode();
RankedSearchResult result = index.AI.Search("how to end a rental agreement early");
```

An index gives up to 64 texts in a call of the [Embed](https://reference.groupdocs.com/search/net/groupdocs.search.ai/itextembedder/embed/) method. If the service limits the number of the texts of a request, send them in parts.

## What to expect

*   **Search between languages.** On a test set, a question in one language has found its paragraph in another language among the first ten results for 88 to 99 percent of the questions, with the word vectors model and all the channels enabled. The channels of the words do not search between languages at all.
*   **Search within one language.** The vectors complement the words: the text channel together with the word vectors has found the right paragraph first for 88 to 93 percent of the questions, against 80 to 92 percent of the text channel alone.
*   **Size.** A vector of 512 components takes about 0.58 KB of the index for a passage.
*   **Speed of a search.** In an index of up to 32,768 passages, a query is compared with the vector of every passage. In a larger index, the candidates are selected by compact codes of the vectors first, and the best of them are compared exactly.

## Notes

*   An index remembers the identifier of the model of vectors and the number of the components, and accepts only that model afterwards, see [Changing the model]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md#changing-the-model" >}}).
*   An exception of the model is thrown by the operation that has called it: the [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) or the [Search](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/search/) method.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
