---
id: ai-search
url: search/net/ai-search
title: AI search
weight: 4
description: "AI search in GroupDocs.Search for .NET: ranked search by meaning with neural terms and passage vectors, reranking, answers with citations, translation of requests into queries, document categories and synonym suggestions."
keywords: AI search, semantic search, ranked search, neural sparse search, vector search, hybrid search, reranking, RAG, answers with citations, natural language query, document classification, local models, ONNX
productName: GroupDocs.Search for .NET
hideChildren: False
---
Since version 26.12, GroupDocs.Search finds documents not only by the words of a query, but also by its meaning, and uses language models to answer questions by the documents, to build queries and to enrich an index. The articles of this section describe these functions.

All the functions are optional and are enabled explicitly. They are provided by the [AI](https://reference.groupdocs.com/search/net/groupdocs.search/index/ai/) property of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class, which returns an instance of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class, and by the types of the `GroupDocs.Search.AI` namespace. The existing API is not changed: an application that does not use the AI search gets the same results as before and no new dependencies.

The models that find documents by meaning run locally, on your computer: the text of the documents is not sent anywhere. Only the functions of a language model give texts to the model that the application connects, see [Privacy](#privacy).

## Functions

| Function | What it gives | What it needs |
| --- | --- | --- |
| [Ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}) | The documents ordered by the relevance to a query in a natural language, with the best passages of their text. | Nothing: it works in any index. |
| [Search by neural terms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-neural-terms.md" >}}) | The ranked search finds the documents that are about the query, even if they have other forms of its words or other words. | A neural terms model and ONNX Runtime, for the encoding of the documents only. |
| [Search by passage vectors]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-passage-vectors.md" >}}) | The ranked search finds the documents by the meaning of the query, also when the query and the documents are in different languages. | A model of vectors: a small model that needs no runtime, a neural network or a service. |
| [Reranking of passages]({{< ref "search/net/developer-guide/advanced-usage/ai-search/reranking-passages.md" >}}) | A model reads the query together with each of the best passages and orders them more exactly. | A reranker and ONNX Runtime. |
| [Semantic expansion of queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/semantic-query-expansion.md" >}}) | The usual search finds the words that are close in meaning to the words of a query: forms, synonyms and translations. | A word vectors model. |
| [Translating requests into queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/natural-language-queries.md" >}}) | A request of a user in plain words becomes a query in the query language of the index. | A language model. |
| [Answers with citations]({{< ref "search/net/developer-guide/advanced-usage/ai-search/answers-with-citations.md" >}}) | An answer to a question by the documents of the index, in which every statement is confirmed by a quote that is found in a document. | A language model. |
| [Sorting documents into categories]({{< ref "search/net/developer-guide/advanced-usage/ai-search/document-categories.md" >}}) | The documents get attributes of the categories that the application describes in words. | The AI data of the index, a language model, or both. |
| [Suggesting synonyms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/synonym-suggestions.md" >}}) | Synonyms of the words of the index for the synonym dictionary, checked by the index and reviewed by a person. | A language model. |

The measured quality and speed of these functions are given in [Quality and performance of AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-quality.md" >}}).

## What to connect

### Packages

| Package | What it has | When it is needed |
| --- | --- | --- |
| GroupDocs.Search | All the functions of the AI search, the tokenizers and the models themselves, and the client of the services of language models. | Always. It has no new dependencies. |
| GroupDocs.Search.AI.Onnx | The [OnnxModelRuntime](https://reference.groupdocs.com/search/net/groupdocs.search.ai.onnx/onnxmodelruntime/) class, which runs the neural networks of the models by ONNX Runtime. | To encode the documents by a neural terms model, and to use a neural model of vectors or a reranker. |

See [Installation]({{< ref "search/net/getting-started/installation.md#packages-of-the-ai-search" >}}) for the dependencies of the packages.

### Models

The models that find documents by meaning are files that the application gives to the library as model packages, see [Models for AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md" >}}). A language model is a component of the application: a local model, a service of your company or a cloud service, see [Language models]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md" >}}).

### Stages

The functions can be enabled step by step, as the application is ready to connect more:

| Stage | What is connected | What works |
| --- | --- | --- |
| 0 | Only GroupDocs.Search. | The order of the results of the usual search by relevance ([BM25]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md#resultorder-property" >}})); the ranked search by the text channel with passages. |
| 1 | A word vectors model, a file of 55 MB. No runtime is needed. | The ranked search by meaning and between languages; the semantic expansion of queries; the sorting of documents into categories without a language model. |
| 2 | The GroupDocs.Search.AI.Onnx package, ONNX Runtime and a neural terms model. | The neural terms channel, which gives the best quality within one language. With the same runtime: a neural model of vectors instead of the word vectors model, and a reranker. |
| 3 | A language model. | Translation of requests into queries, answers with citations, sorting of the documents that the index channels are not sure about, suggestion of synonyms. |

Stage 3 does not need stages 1 and 2: the answers with citations work in any index that stores the text of its documents.

## Before you start

Most functions of the AI search read the text of the documents from the index: the encoding of the documents, the passages of the text channel, the answers with citations, the sorting by a language model and the suggestion of synonyms. Therefore, create the index with the [text storage]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}) enabled. The setting is fixed when an index is created, so an existing index that does not store the text has to be created again.

The following example creates such an index and performs the ranked search without any model.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index that stores the text of the documents
IndexSettings settings = new IndexSettings();
settings.TextStorageSettings = new TextStorageSettings(Compression.High);
Index index = new Index(indexFolder, settings);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Searching by a query in a natural language
RankedSearchOptions options = new RankedSearchOptions();
options.TextPassageDocumentCount = 10; // The first 10 documents get the best passages of their text
RankedSearchResult result = index.AI.Search("how to end a rental agreement early", options);

// Printing the found documents, the most relevant first
for (int i = 0; i < result.DocumentCount; i++)
{
    RankedDocument document = result.GetFoundDocument(i);
    Console.WriteLine(document.DocumentInfo.FilePath);
    if (document.PassageCount > 0)
    {
        Console.WriteLine("    " + document.GetPassage(0).Text);
    }
}
```

## Privacy

*   The neural terms models, the models of vectors and the rerankers of the library run in the process of the application. They read the texts of the documents and of the queries and send nothing.
*   A language model is an object that the application sets by the [LanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/languagemodel/) property. If it is a remote service, the texts that the library gives to it leave the computer. Each function gives the model only what it needs: [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) gives the request of the user, the names of the fields and of the attributes, and the current date; [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/) gives the question and the best passages of the found documents; [Enrich](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/enrich/) gives the name and the beginning of the text of a document, and only if the application asks for the language model; [SuggestSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/suggestsynonyms/) gives words of the index without their texts.
*   The same is true of a model of vectors or a reranker that the application implements over a remote service: it gets the texts of the passages and of the queries.
*   The filters of the options limit the documents whose passages a model gets, so an application can answer a user only by the documents that the user may read.

## Limitations

*   The [search network]({{< ref "search/net/developer-guide/advanced-usage/scaling/_index.md" >}}) and the [index repository]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-repository.md" >}}) do not provide the functions of the AI search in version 26.12.
*   The evaluation version limits the number of the encoded documents and of the documents of a result, see [Evaluation limitations]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md#ai-search-limitations" >}}).
*   An index whose documents are encoded cannot be opened by version 26.11 and earlier versions, see [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#indexes-of-version-2612" >}}).

## Articles of the section

### Search by meaning

*   [Ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}})
*   [Models for AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md" >}})
*   [Encoding documents]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md" >}})
*   [Search by neural terms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-neural-terms.md" >}})
*   [Search by passage vectors]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-passage-vectors.md" >}})
*   [Reranking of passages]({{< ref "search/net/developer-guide/advanced-usage/ai-search/reranking-passages.md" >}})
*   [Semantic expansion of queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/semantic-query-expansion.md" >}})

### Functions of a language model

*   [Language models]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md" >}})
*   [Translating requests into queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/natural-language-queries.md" >}})
*   [Answers with citations]({{< ref "search/net/developer-guide/advanced-usage/ai-search/answers-with-citations.md" >}})
*   [Sorting documents into categories]({{< ref "search/net/developer-guide/advanced-usage/ai-search/document-categories.md" >}})
*   [Suggesting synonyms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/synonym-suggestions.md" >}})

### Measurements

*   [Quality and performance of AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-quality.md" >}})

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
