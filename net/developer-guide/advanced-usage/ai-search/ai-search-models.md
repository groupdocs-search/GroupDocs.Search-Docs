---
id: ai-search-models
url: search/net/ai-search-models
title: Models for AI search
weight: 2
description: "This article describes the models that GroupDocs.Search for .NET uses for the search by meaning: the roles of the models, the tested models, model packages and their preparation, and the ONNX Runtime adapter that runs neural networks."
keywords: model package, neural sparse model, embedding model, reranker, ONNX Runtime, OnnxModelRuntime, multilingual-e5-small, static embeddings, CUDA, DirectML
productName: GroupDocs.Search for .NET
hideChildren: False
---
The search by meaning uses models: files with the knowledge that a neural network has learned from a large collection of texts. GroupDocs.Search runs the models locally, in the process of your application. This article describes which models the library uses, how a model is given to the library and what runs it.

The models are works of their authors, published under open licenses. They are not a part of GroupDocs.Search and are delivered separately from it.

## Roles of models

A model has one of four roles, the values of the [ModelRole](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelrole/) enumeration:

| Role | What the model does | What runs it | How it is given to an index |
| --- | --- | --- | --- |
| [NeuralTerms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelrole/) | Reads a passage of a document and returns the words that the passage is about, with weights, see [Search by neural terms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-neural-terms.md" >}}). | A runtime, during the [encoding of the documents]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md" >}}) only. A search does not run the network. | The [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method. |
| [WordVectors](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelrole/) | Keeps a vector for every word of its vocabulary. The vector of a text is the mean of the vectors of its words. It serves the [passage vectors channel]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-passage-vectors.md" >}}) and the [semantic expansion of queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/semantic-query-expansion.md" >}}). | The library itself. No runtime and no neural network are needed. | The [WordVectorEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/wordvectorembedder/) class for the vectors of passages; the [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method for the semantic expansion. |
| [PassageVectors](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelrole/) | A neural network that reads a text and returns its vector. | A runtime, during the encoding and for every query of a search. | The [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method or the [NeuralTextEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/neuraltextembedder/) class. |
| [Reranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelrole/) | A neural network that reads a query and a passage together and returns how well the passage answers the query, see [Reranking of passages]({{< ref "search/net/developer-guide/advanced-usage/ai-search/reranking-passages.md" >}}). | A runtime, in the searches that use the reranker. | The [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method or the [NeuralReranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/neuralreranker/) class. |

A model of vectors or a reranker can also be any object of the application that implements the [ITextEmbedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/itextembedder/) or the [IReranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ireranker/) interface, for example a client of a service.

## Models that the library is tested with

| Model | Role | Size | License |
| --- | --- | --- | --- |
| [opensearch-neural-sparse-encoding-multilingual-v1](https://huggingface.co/opensearch-project/opensearch-neural-sparse-encoding-multilingual-v1) | NeuralTerms | 670 MB, or 174 MB with 8-bit weights | Apache-2.0 |
| [static-similarity-mrl-multilingual-v1](https://huggingface.co/sentence-transformers/static-similarity-mrl-multilingual-v1) | WordVectors | 55 MB | Apache-2.0 |
| [multilingual-e5-small](https://huggingface.co/intfloat/multilingual-e5-small) | PassageVectors | 470 MB | MIT |
| [mmarco-mMiniLMv2-L12-H384-v1](https://huggingface.co/cross-encoder/mmarco-mMiniLMv2-L12-H384-v1) | Reranker | 471 MB | Apache-2.0 |

All four models are multilingual. The license of a model is the license of its authors: read it before you distribute the model with your application.

Other models can be prepared as well, if the library has their tokenizer:

*   A model of the PassageVectors or the Reranker role needs a tokenizer of the WordPiece type (the models of the BERT family) or of the Unigram type of SentencePiece (XLM-RoBERTa and the models built on its vocabulary, such as multilingual-e5, mMiniLM and bge-m3), and a network in the ONNX format.
*   A model of the WordVectors role is a static embedding model with a WordPiece vocabulary.

### Neural terms model with 8-bit weights

The neural terms model has two variants of its network. The variant with 8-bit weights is four times smaller and encodes the documents faster on a processor. On the test queries it has given the same first passage as the full variant for 14 queries of 14, and the weights of the terms of a passage differ a little: the cosine between the two encodings of a passage is 0.996 on average. The 8-bit variant is meant for the encoding on a processor.

The two variants have the same identifier, version and vocabulary, so an index that is encoded by one of them accepts the other. The numbers of an 8-bit network depend slightly on the version of ONNX Runtime and on the processor, so two computers may give a passage slightly different weights. This does not affect the order of the documents noticeably.

## Model packages

A model is given to the library as a model package: a folder that has the files of the model in the form that the library reads, and the `model.json` file that describes them. The [Load](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelpackage/load/) method of the [ModelPackage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelpackage/) class reads a package.

**C#**

```csharp
string modelFolder = @"c:\Models\static-similarity-mrl-multilingual-v1";

// Reading the description of the model; the sizes of the files are checked
ModelPackage package = ModelPackage.Load(modelFolder);
Console.WriteLine(package.Id + " " + package.Version + ", " + package.Role + ", " + package.License);

// Checking the files of the model by their checksums
package.Verify();
```

The [Load](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelpackage/load/) method checks that the files of the package exist and have the declared sizes. The [Verify](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelpackage/verify/) method reads all the files and compares their SHA-256 checksums with the description; it throws [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception) if a file is changed. The files that an index reads itself, such as a vocabulary, are verified when they are read. Call [Verify](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelpackage/verify/) once after a package is copied or downloaded: the file of a network takes a second or more to read.

A package of the tested models has the following files:

| File | Content |
| --- | --- |
| `model.json` | The description: the identifier, the version, the role and the license of the model, the settings of the tokenizer, the description of the network, and the list of the files with their sizes and checksums. |
| `vocab.txt` or `vocabulary.tsv` | The vocabulary of the tokenizer. |
| `normalization.bin` | The table of the normalization of the text, for a tokenizer of the Unigram type. |
| `model.onnx` | The neural network in the ONNX format. A word vectors model has no network. |
| `query_weights.txt` | The weights of the tokens of a query, for a neural terms model. |
| `vectors.bin` | The vectors of the words, for a word vectors model. |
| `NOTICE.txt` | The origin of the model, its license and the changes made to its files. |

The description of a model looks as follows; the settings of the tokenizer and the list of the files are shortened.

```json
{
  "format": 1,
  "id": "multilingual-e5-small",
  "version": "1.0.0",
  "role": "PassageVectors",
  "license": "MIT",
  "source": "https://huggingface.co/intfloat/multilingual-e5-small",
  "tokenizer": { "type": "Unigram", "vocabulary": "vocabulary.tsv", "normalization": "normalization.bin" },
  "network": {
    "format": "onnx",
    "file": "model.onnx",
    "tokenInput": "input_ids",
    "maskInput": "attention_mask",
    "segmentInput": "token_type_ids",
    "output": "embedding",
    "outputSize": 384,
    "maxTokenCount": 512
  },
  "passageVectors": { "queryPrefix": "query: ", "documentPrefix": "passage: " },
  "files": [
    { "name": "model.onnx", "size": 470268950, "sha256": "1b6656564602cad9..." }
  ]
}
```

The identifier and the version of the package identify the model for an index: an index that is encoded by a model accepts only that model afterwards, see [Encoding documents]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md#changing-the-model" >}}).

## Preparing model packages

A model package is made from the files that the authors of the model publish on Hugging Face. The Python scripts that make the packages of the tested models are published in the `Models` folder of the [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET) repository. A script downloads nothing by itself except the files of the model, does not train the model and does not change the weights of its network. It writes the tokenizer in the form that the library reads, completes the network so that it returns one row of numbers for a text, compares the result with the source model on sample texts, and writes the folder of the package with a report.

The scripts need Python 3.10 or later. Run them in a folder of their own: the downloaded models are saved to its `hf` subfolder, and the packages are written to its `packages` subfolder.

```
pip install -U huggingface_hub numpy onnx onnxruntime safetensors tokenizers
```

The neural terms model with 8-bit weights and the word vectors model:

```
python download_models.py
python export_sparse_onnx.py --int8
python build_model_package.py --variant int8
python build_word_vectors_package.py
```

The neural model of vectors and the reranker:

```
python download_dense_models.py
python build_dense_package.py --model hf/multilingual-e5-small --pooling mean --max-tokens 512 --license MIT --query-prefix "query: " --document-prefix "passage: " --source https://huggingface.co/intfloat/multilingual-e5-small
python build_dense_package.py --model hf/mmarco-mMiniLMv2-L12-H384-v1 --max-tokens 512 --license Apache-2.0 --source https://huggingface.co/cross-encoder/mmarco-mMiniLMv2-L12-H384-v1
```

Prepare only the models of the [stages]({{< ref "search/net/developer-guide/advanced-usage/ai-search/_index.md#stages" >}}) that you use: the word vectors model alone is enough for the search by meaning. The header of each script describes its options, including the options for another model of the same kind.

A package is a plain folder. Copy it to the computers of your application as it is, with the `NOTICE.txt` file and the license of the model.

## ONNX Runtime adapter

The library does not run neural networks itself. A network is run by a runtime: an object that implements the [IModelRuntime](https://reference.groupdocs.com/search/net/groupdocs.search.ai/imodelruntime/) interface and is set by the [Runtime](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/runtime/) property of the index. The GroupDocs.Search.AI.Onnx package provides such an object, the [OnnxModelRuntime](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxmodelruntime/) class of the `GroupDocs.Search.AI.Onnx` namespace, which runs the networks by [ONNX Runtime](https://onnxruntime.ai/).

The package references only the managed part of ONNX Runtime. The application adds the package with the native library for its device:

| Device | NuGet package of the application | Value of [OnnxExecutionProvider](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxexecutionprovider/) |
| --- | --- | --- |
| Processor | Microsoft.ML.OnnxRuntime | [Cpu](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxexecutionprovider/), the default |
| Video card of NVIDIA through CUDA | Microsoft.ML.OnnxRuntime.Gpu, with the libraries of CUDA and cuDNN | [Cuda](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxexecutionprovider/) |
| Video card of any vendor through DirectML, on Windows | Microsoft.ML.OnnxRuntime.DirectML | [DirectML](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxexecutionprovider/) |

The version of ONNX Runtime must be 1.22.1 or later.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\opensearch-neural-sparse-encoding-multilingual-v1-int8";

// Opening an index that stores the text of its documents
Index index = new Index(indexFolder);

// The runtime computes on the processor by default
OnnxModelRuntime runtime = new OnnxModelRuntime();
runtime.ThreadCount = 4; // The number of the threads of a network; 0 means all the cores

// To compute on the first video card through CUDA instead:
// runtime.ExecutionProvider = OnnxExecutionProvider.Cuda;
// runtime.DeviceId = 0;

index.AI.Runtime = runtime;
index.AI.AddModel(ModelPackage.Load(modelFolder));
index.AI.Encode();
```

The properties of the [OnnxModelRuntime](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxmodelruntime/) class:

| Property | Description | Default |
| --- | --- | --- |
| [ExecutionProvider](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxmodelruntime/executionprovider/) | The device that a network is computed on. | Cpu |
| [DeviceId](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxmodelruntime/deviceid/) | The number of the video card for CUDA and DirectML. | 0 |
| [ThreadCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxmodelruntime/threadcount/) | The number of the threads that compute a network on the processor; 0 leaves the choice to ONNX Runtime, which uses all the cores. | 0 |
| [BatchTokenCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxmodelruntime/batchtokencount/) | The greatest number of the tokens of the sequences that a network computes in one run; 0 means the choice by the device: a processor computes every sequence alone, a video card computes sequences together within 4096 tokens. | 0 |
| [ConfigureSession](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxmodelruntime/configuresession/) | A method that changes the session options of ONNX Runtime before a network is loaded: for example, it adds another execution provider. | null |

The properties are read when a network is opened, and a change does not affect the networks that are already open. One runtime object can serve several indexes and several models.

If the native library of ONNX Runtime is not found or does not support the chosen device, the operation that first needs the network throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception).

### Which operations need the runtime

| Operation | Neural terms model | Word vectors model | Neural model of vectors | Reranker |
| --- | --- | --- | --- | --- |
| Encoding of the documents | Runtime | No runtime | Runtime | Not used |
| Ranked search | Not needed: the query is encoded by a table that the index keeps | No runtime; the model converts the query | Runtime; the model converts the query | Runtime, if the search uses the reranker |

Therefore, an index that is encoded by a neural terms model, alone or with a word vectors model, is searched on a computer that has neither ONNX Runtime nor the file of the network.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
