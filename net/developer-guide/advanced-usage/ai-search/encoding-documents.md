---
id: encoding-documents
url: search/net/encoding-documents
title: Encoding documents
weight: 3
description: "This article describes how GroupDocs.Search for .NET encodes the documents of an index by models for the search by meaning: the Encode method, its options, progress and cancellation, new and changed documents, and the removal of the AI data."
keywords: encode documents, AI data of an index, neural terms, passage vectors, EncodingOptions, AIIndexInfo, RemoveData
productName: GroupDocs.Search for .NET
hideChildren: False
---
To find documents by meaning, an index needs data that a model makes from the text of the documents: the neural terms and the vectors of the passages. Making this data is called encoding, and the data is called the AI data of the index. The documents are encoded once by the [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class; after that, the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}) uses the data in every search.

The encoding reads the text that the index [stores]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}). The files of the documents are not read, so they do not have to be available. An index that does not store the text of its documents cannot be encoded.

## Encoding

The documents are encoded by the models that are set at the moment of the call:

*   a neural terms model, added by the [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method and run by the runtime of the [Runtime](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/runtime/) property, see [Search by neural terms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-neural-terms.md" >}});
*   a model of vectors, set by the [Embedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/embedder/) property or added by the [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method, see [Search by passage vectors]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-passage-vectors.md" >}}).

One of the models is enough. The following example encodes the documents by both.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string neuralTermsFolder = @"c:\Models\opensearch-neural-sparse-encoding-multilingual-v1-int8";
string wordVectorsFolder = @"c:\Models\static-similarity-mrl-multilingual-v1";

// Creating an index that stores the text of the documents
IndexSettings settings = new IndexSettings();
settings.TextStorageSettings = new TextStorageSettings(Compression.High);
Index index = new Index(indexFolder, settings);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Setting the models: the neural terms model needs a runtime, the word vectors model does not
index.AI.Runtime = new OnnxModelRuntime();
index.AI.AddModel(ModelPackage.Load(neuralTermsFolder));
index.AI.Embedder = new WordVectorEmbedder(ModelPackage.Load(wordVectorsFolder));

// Encoding the documents that are not encoded yet
index.AI.Encode();

// The ranked search uses the channels that the index has data for
RankedSearchResult result = index.AI.Search("how to end a rental agreement early");
```

The models are not kept in the index. When an index is opened again, set the models that its searches need: the model of vectors, because it converts every query into a vector, and the models that will encode new documents. The neural terms channel is searched without any model.

## New, changed and deleted documents

The [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method encodes only the documents that are not encoded yet. Call it again after documents are added or updated: until then, the new documents are found by the text channel of the ranked search.

| Operation | What happens to the AI data |
| --- | --- |
| Adding a new document | The document waits for the encoding. The text channel finds it at once. |
| Adding or updating a changed document | The data of the previous version is not found any more; the new version waits for the encoding. |
| [Deleting]({{< ref "search/net/developer-guide/advanced-usage/indexing/delete-indexed-documents.md" >}}) a document | The document is not found. Its data is removed by the next optimization. |
| [Optimizing]({{< ref "search/net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}) the index | The segments of the AI data are merged and the data of the deleted documents is removed. The fewer segments the data has, the faster the ranked search is. |
| [Merging]({{< ref "search/net/developer-guide/advanced-usage/indexing/merge-indexes.md" >}}) another index into the index | The AI data of the target index is kept. The documents of the merged index wait for the encoding, even if they were encoded in their own index. |
| [Checking]({{< ref "search/net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}}) the index | The files of the AI data are checked. Damaged data of a channel is excluded, and its documents wait for the encoding again. |
| Saving the index to another folder, loading it into memory | The AI data goes with the index. |

## Options of the encoding

The [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method has an overload that takes an instance of the [EncodingOptions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/encodingoptions/) class:

| Property | Description | Default |
| --- | --- | --- |
| [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/encodingoptions/cancellation/) | The object for cancelling the operation. | null |
| [MaxDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/encodingoptions/maxdocumentcount/) | The greatest number of the documents that a call encodes; 0 means no limit. A file of an archive counts as a document. | 0 |
| [Threads](https://reference.groupdocs.com/search/net/groupdocs.search.ai/encodingoptions/threads/) | The number of the threads that read the stored text and divide it into passages while the model encodes the passages that are ready, from 1 to 64. The result does not depend on the number. The network itself is computed by the threads of the runtime, see [ThreadCount]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md#onnx-runtime-adapter" >}}). | 1 |

The encoding by a neural network is a long operation, see [Speed and size](#speed-and-size). It saves its work in parts, so it can be stopped and continued:

*   A cancelled call, or a call limited by [MaxDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/encodingoptions/maxdocumentcount/), keeps the documents that are encoded completely. The next call continues with the others.
*   If a model or the runtime fails, the exception of the failure is thrown by the [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method, and the documents encoded before it are kept.

The following example encodes the documents for at most two hours and reports the progress.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\opensearch-neural-sparse-encoding-multilingual-v1-int8";

// Opening an index that stores the text of its documents
Index index = new Index(indexFolder);
index.AI.Runtime = new OnnxModelRuntime();
index.AI.AddModel(ModelPackage.Load(modelFolder));

// Subscribing to the events of the index
index.Events.OperationProgressChanged += (sender, args) =>
{
    Console.WriteLine(args.ProcessedDocuments + " of " + args.TotalDocuments + ": " + args.LastDocumentPath);
};
index.Events.ErrorOccurred += (sender, args) =>
{
    Console.WriteLine(args.Message);
};

// Encoding with a limit of time
using (Cancellation cancellation = new Cancellation())
{
    cancellation.CancelAfter(2 * 60 * 60 * 1000); // Two hours, in milliseconds

    EncodingOptions options = new EncodingOptions();
    options.Threads = 2;
    options.Cancellation = cancellation;
    index.AI.Encode(options);
}

// The documents that are left wait for the next call
AIIndexInfo info = index.AI.GetInfo();
Console.WriteLine("Encoded: " + info.EncodedDocumentCount + ", waiting: " + info.PendingDocumentCount);
```

### Progress, errors and other operations

*   The progress is reported by the [OperationProgressChanged](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/operationprogresschanged/) event for every document, and the end of the operation by the [OperationFinished](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/operationfinished/) event with the [OperationType.Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.events/operationtype/) type of operation.
*   A document whose stored text cannot be read is reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event. It is left waiting for the next encoding, and the other documents are encoded.
*   During the encoding, the status of the index is [IndexStatus.Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexstatus/). Searches are performed as usual. Another operation that changes the index, such as the indexing or the optimization, is not performed at the same time, as two indexing operations are not.

## Information about the AI data

The [GetInfo](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/getinfo/) method returns an instance of the [AIIndexInfo](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/) class, which describes the AI data of the index at the moment of the call:

| Property | Description |
| --- | --- |
| [HasData](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/hasdata/) | Whether the documents of the index have been encoded. |
| [Channels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/channels/) | The channels of the ranked search that the index has data for, besides the text channel, which every index has. |
| [NeuralTermsModelId](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/neuraltermsmodelid/), [NeuralTermsModelVersion](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/neuraltermsmodelversion/) | The identifier and the version of the neural terms model of the index, or null. |
| [PassageVectorsModelId](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/passagevectorsmodelid/), [PassageVectorsDimension](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/passagevectorsdimension/) | The identifier of the model of vectors of the index and the number of the components of a vector, or null and 0. |
| [EncodedDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/encodeddocumentcount/) | The number of the documents that have the data of every channel of the index. |
| [PendingDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/pendingdocumentcount/) | The number of the documents that wait for the encoding. |
| [PassageCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/passagecount/) | The number of the passages of the encoded documents. |
| [SegmentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/segmentcount/) | The number of the segments of the AI data. |

Use the [PendingDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/aiindexinfo/pendingdocumentcount/) property to decide whether the [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method has to be called, and the identifiers of the models to set the right models for an index that is opened.

## Adding a channel later

An index can be encoded by one model first and by the other later. When a model of the other channel is set for an encoded index, the [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method gives the data of that channel to the encoded documents and does not compute the data that they already have. A call with one model of an index that has two channels encodes that channel only, and the documents wait for the other model.

## Changing the model

The data of all the documents of an index has to be made in the same way: the neural terms of two models, or the vectors of two models, cannot be compared. Therefore, an index remembers the models that have encoded its documents and accepts only them afterwards:

*   the neural terms model, by its identifier, version, vocabulary and settings;
*   the model of vectors, by the [ModelId](https://reference.groupdocs.com/search/net/groupdocs.search.ai/itextembedder/modelid/) and [Dimension](https://reference.groupdocs.com/search/net/groupdocs.search.ai/itextembedder/dimension/) properties.

The [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if a model differs from the model of the index. A search passes over the passage vectors channel if the model of vectors that is set is not the model of the index.

To change a model, remove the AI data and encode the documents again.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\multilingual-e5-small";

// Opening an encoded index
Index index = new Index(indexFolder);

// Removing the AI data: the documents and the usual search are not affected
index.AI.RemoveData();

// Encoding the documents by another model of vectors
index.AI.Runtime = new OnnxModelRuntime();
index.AI.AddModel(ModelPackage.Load(modelFolder));
index.AI.Encode();
```

The [RemoveData](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/removedata/) method removes only the AI data. After the call, the index is saved exactly as an index whose documents have never been encoded, so version 26.11 of the library opens it again, see [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#indexes-of-version-2612" >}}).

## How the text is divided into passages

*   The text of every field of a document is divided into passages of about a thousand characters. The rules of the division are fixed when the documents of an index are first encoded.
*   The name of the file of a document, without the folders and the extension, is given to a model before the text of every passage, so that a passage from the middle of a document does not lose its subject. Give the documents meaningful names: they help the search by meaning.
*   Equal passages, for example of the copies of a document in different folders, are given to a model once.

## Speed and size

The time of the encoding is the time of the models:

*   A neural network is slow on a processor. The neural terms model with 8-bit weights has encoded about 14 passages of 128 tokens in a second on a desktop processor with 6 cores, with about 750 MB of memory: a collection of 100,000 passages takes two or three hours.
*   The word vectors model is not a neural network: it encodes a thousand passages or more in a second on one processor core.

The AI data takes about 0.55 KB for the neural terms and 0.58 KB for a vector of 512 components per passage. See [Quality and performance of AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-quality.md" >}}) for the measurements.

## Notes

*   The evaluation version encodes at most 100 documents of an index, see [Evaluation limitations]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md#ai-search-limitations" >}}).
*   Damaged AI data does not prevent an index from opening: the usual search and the text channel of the ranked search work, and the documents can be encoded again.
*   The [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if no model is set, if the runtime is not set for a model that needs it, if the index does not store the text of its documents, if the index is encoded by another model, or if the index performs another operation.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
