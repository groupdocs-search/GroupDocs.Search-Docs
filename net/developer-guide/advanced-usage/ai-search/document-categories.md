---
id: document-categories
url: search/net/document-categories
title: Sorting documents into categories
weight: 11
description: "This article describes how GroupDocs.Search for .NET sorts the documents of an index into categories that an application describes in words, and gives the documents attributes of the categories: by the AI data of the index without a language model, by a language model, or by both."
keywords: document classification, document categories, zero-shot classification, document attributes, Enrich, EnrichmentOptions, language model, faceted search
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [Enrich](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/enrich/) method of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class sorts the documents of an index into categories and gives every sorted document the attribute of its category. The application describes the categories in words, for example "Contracts: agreements between parties, obligations, terms, signatures". No examples and no training are needed.

The attributes are ordinary [document attributes]({{< ref "search/net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}). After the sorting, any search can be limited to a category by a [document filter]({{< ref "search/net/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}): the usual search, the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}) and the [answers with citations]({{< ref "search/net/developer-guide/advanced-usage/ai-search/answers-with-citations.md" >}}). The attributes can be changed by hand as any other attributes.

## Who judges the documents

A document can be judged in two ways, and the options choose one of them or both:

| | Index channels | Language model |
| --- | --- | --- |
| How | The neural terms and the vectors of the passages of a document are compared with the descriptions of the categories and with the documents that are already sorted. | The model reads the descriptions of the categories, the name of a document and the beginning of its text, and names the category. |
| What it needs | The documents [encoded]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md" >}}) by a neural terms model, a model of vectors, or both. | A [language model]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md" >}}) and the text of the documents stored in the index. |
| Speed | Fractions of a second for thousands of documents. | A request to the model for a document. |
| Privacy | Nothing leaves the computer. | The name and the beginning of the text of a document are given to the model. |
| Quality | A decision is approximate and has a confidence. The documents of clearly different subjects are sorted reliably; the documents of close categories are left without a decision. | Depends on the model. |
| Option | [UseIndexChannels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/useindexchannels/), true by default | [UseLanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/uselanguagemodel/), false by default |

When both are enabled, the index channels judge first, and the language model gets only the documents that the channels have not sorted confidently, starting with the least confident ones, and then the documents that are not encoded. So most documents are sorted in a moment and without sending their text anywhere, and the model is asked only about the difficult ones.

A decision of the channels is accepted if its confidence is not less than the [MinConfidence](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/minconfidence/) option, 0.8 by default.

## Sorting by the index channels

The following example sorts the documents of an encoded index without a language model and searches within a category.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\static-similarity-mrl-multilingual-v1";

// Opening an encoded index and setting its model of vectors
Index index = new Index(indexFolder);
index.AI.Embedder = new WordVectorEmbedder(ModelPackage.Load(modelFolder));

// Describing the categories: an attribute and a description in words
EnrichmentOptions options = new EnrichmentOptions();
options.AddCategory("type:contract", "Contracts: agreements between parties, obligations, terms, signatures");
options.AddCategory("type:invoice", "Invoices: bills, amounts due, payment details");
options.AddCategory("type:report", "Reports: results of a period, analysis, figures, conclusions");

// Sorting the documents
EnrichmentReport report = index.AI.Enrich(options);
Console.WriteLine("Documents: " + report.DocumentCount);
Console.WriteLine("Sorted: " + report.ClassifiedDocumentCount);
Console.WriteLine("Not sorted, the decision is not confident: " + report.UnclassifiedDocumentCount);
Console.WriteLine("Not examined, for example not encoded: " + report.PendingDocumentCount);
for (int i = 0; i < report.CategoryCount; i++)
{
    Console.WriteLine(report.GetCategory(i).Attribute + ": " + report.GetCategoryDocumentCount(i));
}

// Searching in the documents of a category
SearchOptions searchOptions = new SearchOptions();
searchOptions.SearchDocumentFilter = SearchDocumentFilter.CreateAttribute("type:contract");
SearchResult result = index.Search("termination OR notice", searchOptions);
```

Notes on the index channels:

*   At least two categories are needed: a document is sorted by the difference between the categories.
*   The channel of the vectors works if the model of vectors of the index is set by the [Embedder](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/embedder/) property, as in a search. The neural terms need no model.
*   A description of a category works best when it names the category and lists the words that are typical of its documents.

## Sorting with a language model

Set the [UseLanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/uselanguagemodel/) option to give the documents that are left to the language model of the index. The model replies with the number of a category, or tells that a document belongs to none of them.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index that stores the text of its documents
Index index = new Index(indexFolder);
index.AI.LanguageModel = new ChatCompletionLanguageModel("http://localhost:11434/v1", "model-name");

EnrichmentOptions options = new EnrichmentOptions();
options.AddCategory("type:contract", "Contracts: agreements between parties, obligations, terms, signatures");
options.AddCategory("type:invoice", "Invoices: bills, amounts due, payment details");
options.UnclassifiedAttribute = "type:other"; // For the documents that belong to none of the categories
options.UseLanguageModel = true;
options.MaxLanguageModelDocumentCount = 500; // At most 500 documents are given to the model by this call

EnrichmentReport report = index.AI.Enrich(options);
Console.WriteLine("Sorted: " + report.ClassifiedDocumentCount);
Console.WriteLine("Belong to no category: " + report.UnclassifiedDocumentCount);
Console.WriteLine("Read by the model: " + report.LanguageModelDocumentCount);
Console.WriteLine("Requests to the model: " + report.LanguageModelRequestCount);
```

The address and the name of the model of the example are the ones of a model that a program runs on the same computer; for your service and for a model of another kind, see [Language models]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md#connecting-a-service-by-the-chat-completions-api" >}}).

In an index whose documents are not encoded, the index channels have nothing to judge by. Set the [UseIndexChannels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/useindexchannels/) option to false there, and every document is given to the language model.

## Which documents get attributes

*   A document gets the attribute of a category when the decision of the channels is accepted or the language model has named the category.
*   A document that is examined to the end and left without a category gets the attribute of the [UnclassifiedAttribute](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/unclassifiedattribute/) option, if it is set. This happens when the language model has replied that the document belongs to none of the categories, or, when the language model is not used, when the decision of the channels is not confident.
*   A document that is not examined to the end gets nothing: it is not encoded and the model is not used, the model has not reached it because of a limit or a cancellation, or the model has not replied in the required form. The report counts such documents by the [PendingDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentreport/pendingdocumentcount/) property.

Attributes belong to documents, not to the items of containers: an archive, or an e-mail with attachments, is sorted as one document.

## Running the sorting again

A document that already has the attribute of one of the categories, or the attribute of the [UnclassifiedAttribute](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/unclassifiedattribute/) option, is taken as sorted: it is passed over, and a document of a category serves the index channels as an example of that category. Therefore:

*   After documents are added to the index, the next call with the same options examines only the new documents.
*   A sorting that is stopped by a cancellation, by the [MaxLanguageModelDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/maxlanguagemodeldocumentcount/) limit or by a failure of the service of the model is continued by the next call. While the language model works, the decisions are saved to the index every minute.
*   An attribute that is set or corrected by hand is kept.
*   A few documents that are sorted by hand, by the [ChangeAttributes](https://reference.groupdocs.com/search/net/groupdocs.search/index/changeattributes/) method, improve the decisions of the channels: they are examples of their categories.

To examine all the documents again, without the examples, set the [ReclassifyDocuments](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/reclassifydocuments/) option. The attributes of the options are then replaced: a document keeps only the one that it gets.

## Reviewing the decisions before they are applied

With the [ApplyChanges](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/applychanges/) option set to false, the index is not changed: the report tells what the sorting would do. The [CreateAttributeChanges](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentreport/createattributechanges/) method of the report returns the same changes as a batch for the [ChangeAttributes](https://reference.groupdocs.com/search/net/groupdocs.search/index/changeattributes/) method, so an application can show the decisions to a person and apply them later.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\static-similarity-mrl-multilingual-v1";

// Opening an encoded index and setting its model of vectors
Index index = new Index(indexFolder);
index.AI.Embedder = new WordVectorEmbedder(ModelPackage.Load(modelFolder));

EnrichmentOptions options = new EnrichmentOptions();
options.AddCategory("type:contract", "Contracts: agreements between parties, obligations, terms, signatures");
options.AddCategory("type:invoice", "Invoices: bills, amounts due, payment details");
options.MinConfidence = 0.7;
options.ApplyChanges = false; // Only the report

EnrichmentReport report = index.AI.Enrich(options);

// Printing the decisions: the documents that would get an attribute
foreach (EnrichedDocument document in report)
{
    if (document.Attribute != null)
    {
        string confidence = document.Confidence.ToString("F2");
        Console.WriteLine(document.FilePath + ": " + document.Attribute + ", " + confidence + ", " + document.Method);
    }
}

// Applying the decisions after a review
AttributeChangeBatch batch = report.CreateAttributeChanges();
index.ChangeAttributes(batch);
```

The [EnrichedDocument](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enricheddocument/) class describes the decision on a document:

| Property | Description |
| --- | --- |
| [FilePath](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enricheddocument/filepath/) | The path of the document, or the key of a document that is indexed from a stream or a structure. |
| [Category](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enricheddocument/category/) | The category that suits the document best, or null. The document gets the attribute of the category only if the decision is confident enough. |
| [Attribute](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enricheddocument/attribute/) | The attribute that the document gets, or null if it gets none. |
| [Confidence](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enricheddocument/confidence/) | The confidence of the decision, from 0 to 1. |
| [Method](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enricheddocument/method/) | Who has made the decision: a value of the [EnrichmentMethod](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentmethod/) enumeration. |
| [FailureMessage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enricheddocument/failuremessage/) | The reason why the language model has given no decision for the document, or null. |

### What the confidence means

For a decision of the index channels, the confidence is an estimate of the share of the right decisions among the decisions made with the same margin between the first and the second category. The estimate is fitted on public collections of texts, so on your documents and descriptions the share will differ: use it as a guide for the threshold, not as a guarantee. A decision of a language model has the confidence 1, because a model gives no measure of its own.

On four public collections with known categories, the index channels alone have given the following results with the default threshold:

| Collection | Right decisions among all the documents | Share of the documents sorted with the confidence 0.8 and more | Right among them |
| --- | ---: | ---: | ---: |
| News articles of 5 clearly different subjects | 94.5% | 68% | 99.0% |
| Short news of 4 subjects | 80.9% | 41% | 91.4% |
| German articles of 9 subjects, the descriptions in English | 66.2% | 25% | 86.6% |
| Messages of 20 close subjects | 63.9% | 39% | 86.3% |

See [Quality and performance of AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-quality.md#sorting-documents-into-categories" >}}) for the details.

## Options

| Property | Description | Default |
| --- | --- | --- |
| [AddCategory](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/addcategory/) | Adds a category: the attribute that its documents get and its description in words. | |
| [UnclassifiedAttribute](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/unclassifiedattribute/) | The attribute for the documents that are examined and left without a category. | null: no attribute |
| [UseIndexChannels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/useindexchannels/) | Whether the documents are judged by the AI data of the index. | true |
| [MinConfidence](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/minconfidence/) | The least confidence with which a decision of the index channels is accepted, from 0 to 1. | 0.8 |
| [UseLanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/uselanguagemodel/) | Whether the language model judges the documents that the channels have not sorted confidently. | false |
| [MaxLanguageModelDocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/maxlanguagemodeldocumentcount/) | The greatest number of the documents that one call gives to the language model; 0 means no limit. | 0 |
| [MaxDocumentTextLength](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/maxdocumenttextlength/) | The greatest length of the beginning of the text of a document that the language model gets, from 200 to 200,000 characters. | 4000 |
| [MaxAttemptCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/maxattemptcount/) | The greatest number of the requests to the language model for one document, from 1 to 5. | 2 |
| [AdditionalInstructions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/additionalinstructions/) | The instructions that are added to the instructions of the language model: for example, how to tell two close categories apart. | null |
| [ReclassifyDocuments](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/reclassifydocuments/) | Whether the documents that already have an attribute of the options are examined again. | false |
| [ApplyChanges](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/applychanges/) | Whether the attributes of the documents are changed. | true |
| [SearchDocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/searchdocumentfilter/) | The filter of the documents that are examined, by their paths and attributes. | null: all the documents |
| [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/enrichmentoptions/cancellation/) | The object for cancelling the operation. A cancelled sorting keeps the attributes of the documents that it has sorted. | null |

## Notes

*   While the attributes are changed, the status of the index is [IndexStatus.Enriching](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexstatus/): searches are performed, and another operation that changes the index is not. The progress is reported by the [OperationProgressChanged](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/operationprogresschanged/) event for every document that the language model has read, and the end by the [OperationFinished](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/operationfinished/) event with the [OperationType.Enriching](https://reference.groupdocs.com/search/net/groupdocs.search.events/operationtype/) type of operation.
*   The method throws [ArgumentException](https://learn.microsoft.com/dotnet/api/system.argumentexception) if the options have no categories, if neither the channels nor the language model is used, or if only the channels are used with one category. It throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if the language model is needed and is not set, or if only the channels are used and the documents are not encoded.
*   The evaluation version examines the first 100 documents of the index, see [Evaluation limitations]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md#ai-search-limitations" >}}).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
