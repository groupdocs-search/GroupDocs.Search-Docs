---
id: answers-with-citations
url: search/net/answers-with-citations
title: Answers with citations
weight: 10
description: "This article describes how GroupDocs.Search for .NET answers a question by the documents of an index with a language model: the answer consists only of the statements whose quotes are found in the text of the documents."
keywords: answers with citations, question answering, RAG, retrieval-augmented generation, grounded answers, verified quotes, Ask, language model, LLM
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/) method of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class answers a question by the documents of an index. The index finds the passages that are relevant to the question, a [language model]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md" >}}) writes the answer by them, and the index checks the answer: every statement of the answer has a quote, and every quote is found in the text of a document.

A language model can write a statement that no document supports. The [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/) method does not let such a statement into the answer: the text of the answer is made only of the statements whose quotes the index has found in the documents, and each statement refers to its citation, which the reader can open and check.

## How a question is answered

1.  The question is searched by the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}), with the channels and the filters of the options. The best passages of the found documents become the sources for the model.
2.  The model gets the rules, the sources with the names of their files, and the question. The rules tell it to answer only by the sources, to give the answer as statements, each with a quote that is a part of the text of a source, character by character, and to say that the answer is not found if the sources do not have it. The model is also told not to follow instructions that the texts of the sources may contain.
3.  The index looks for every quote in the text of the sources.
4.  The text of the answer is made of the statements whose quotes are found. A statement whose quote is not found is left out of the text.
5.  If the reply of the model has not the required form, or none of its quotes is found, the model is asked again with a remark.

The method needs only a language model and the [text of the documents stored in the index]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}). It works in an index whose documents are not [encoded]({{< ref "search/net/developer-guide/advanced-usage/ai-search/encoding-documents.md" >}}): the text channel finds the documents, and the passages that have the most of the words of the question are taken. In an encoded index, the question finds the passages by meaning.

## Asking a question

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

// Setting the language model of the index
index.AI.LanguageModel = new ChatClientLanguageModel(CreateChatClient());

// Asking a question
Answer answer = index.AI.Ask("What is the notice period for early termination of the lease?");

if (answer.Status == AnswerStatus.Answered)
{
    // Every statement of the text ends with the number of its citation in square brackets
    Console.WriteLine(answer.Text);

    // The verified citations are the first ones
    for (int i = 0; i < answer.VerifiedCitationCount; i++)
    {
        AnswerCitation citation = answer.GetCitation(i);
        Console.WriteLine("[" + (i + 1) + "] " + citation.DocumentInfo.FilePath);
        Console.WriteLine("    \"" + citation.Quote + "\"");
    }
}
else
{
    Console.WriteLine("No answer: " + answer.Status + " " + answer.FailureMessage);
}
```

The `CreateChatClient` method of the example stands for the code that creates a chat client of your service, see [Language models]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md#connecting-a-model-by-microsoftextensionsai" >}}).

## The answer

The [Answer](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answer/) class represents the result:

| Member | Description |
| --- | --- |
| [Status](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answer/status/) | How the question is answered, see below. |
| [Text](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answer/text/) | The text of the answer: the statements whose quotes are found. Every statement is followed by the number of its citation in square brackets, from 1: the number N refers to the citation with the index N - 1. An empty string if the status is not Answered. |
| [CitationCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answer/citationcount/), [GetCitation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answer/getcitation/) | All the statements of the model with their quotes. The verified citations go first. |
| [VerifiedCitationCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answer/verifiedcitationcount/) | The number of the citations whose quotes are found in the documents: the statements of the text. |
| [SearchResult](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answer/searchresult/) | The result of the ranked search that has found the passages for the model. |
| [FailureMessage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answer/failuremessage/) | The reason why the question is not answered, for the Failed and NotVerified statuses. |
| [AttemptCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answer/attemptcount/) | The number of the requests that the language model has got; 0 if the search has found no passages. |

The statuses of an answer are the values of the [AnswerStatus](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answerstatus/) enumeration:

| Status | Meaning |
| --- | --- |
| [Answered](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answerstatus/) | The answer has at least one statement whose quote is found. |
| [NotFoundInDocuments](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answerstatus/) | The search has found no passages, or the model has replied that the sources do not answer the question. |
| [NotVerified](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answerstatus/) | The model has answered, but none of its quotes is found in the documents in any attempt. The text is empty; the statements of the model are among the citations, marked as not verified. |
| [Failed](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answerstatus/) | The model has not replied in the required form. |
| [Cancelled](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answerstatus/) | The operation is cancelled. |

Treat the NotFoundInDocuments status as an answer, not as an error: the documents of the index do not tell what is asked, and the application can say so to the user.

## Citations

The [AnswerCitation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answercitation/) class represents a statement of the model with its quote:

| Member | Description |
| --- | --- |
| [Statement](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answercitation/statement/) | The statement, as the model has written it. |
| [Quote](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answercitation/quote/) | The quote. A verified quote is the text of the document, character by character. |
| [Verified](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answercitation/verified/) | Whether the quote is found in the text of the document. |
| [DocumentInfo](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answercitation/documentinfo/), [FieldName](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answercitation/fieldname/) | The document and the field of the quote. |
| [Passage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answercitation/passage/), [QuoteIndex](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answercitation/quoteindex/) | The passage that has the quote, one of the passages of the search result, and the position of the quote in its text. |
| [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/answercitation/createquery/) | Creates the query that finds the quote in the index: the phrase of the words of the quote in the field of the quote. |

A quote is found if the passage has its letters and digits in the same order and without gaps, from the start of a word to the end of a word. The case of the letters, the spaces, the line breaks and the punctuation marks are not compared, except the marks between digits: "1,200" is neither "1200" nor "12.00". A quote with omissions is found if the passage has its parts in their order.

A verified quote tells that the document has this text. Whether the text supports the statement is for the reader to judge: this is why the answer gives every statement with its citation.

### Showing a citation in its document

The query of a citation finds the quote by the usual search, so the quote can be [highlighted]({{< ref "search/net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}) in the text of its document as the occurrences of any query.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);
index.AI.LanguageModel = new ChatClientLanguageModel(CreateChatClient());

Answer answer = index.AI.Ask("What is the notice period for early termination of the lease?");
if (answer.VerifiedCitationCount > 0)
{
    AnswerCitation citation = answer.GetCitation(0);

    // Searching for the quote of the citation
    SearchQuery query = citation.CreateQuery();
    SearchResult result = index.Search(query);

    // Highlighting the quote in the text of its document
    for (int i = 0; i < result.DocumentCount; i++)
    {
        FoundDocument document = result.GetFoundDocument(i);
        if (document.DocumentInfo.FilePath == citation.DocumentInfo.FilePath)
        {
            OutputAdapter adapter = new FileOutputAdapter(OutputFormat.Html, @"c:\Output\Citation.html");
            Highlighter highlighter = new DocumentHighlighter(adapter);
            index.Highlight(document, highlighter);
            break;
        }
    }
}
```

## Options

The [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/) method has an overload that takes an instance of the [AskOptions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/) class:

| Property | Description | Default |
| --- | --- | --- |
| [MaxPassageCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/maxpassagecount/) | The greatest number of the passages that the language model gets. More passages let the model find the answer more often and make a request longer. | 8 |
| [MaxPassagesPerDocument](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/maxpassagesperdocument/) | The greatest number of the passages of one document that the model gets, from 1 to 20. | 3 |
| [Channels](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/channels/) | The channels of the ranked search that finds the passages. | All |
| [Filter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/filter/) | The query that limits the documents that the question is answered by. | null |
| [SearchDocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/searchdocumentfilter/) | The filter of the documents by their paths and attributes. | null |
| [UseReranker](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/usereranker/) | Whether the found passages are ordered by the [reranker]({{< ref "search/net/developer-guide/advanced-usage/ai-search/reranking-passages.md" >}}) of the index before the best of them are given to the model. | false |
| [MaxAttemptCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/maxattemptcount/) | The greatest number of the requests to the language model for one question. | 2 |
| [AdditionalInstructions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/additionalinstructions/) | The instructions of the application that are added to the instructions of the library: for example, the language or the manner of the answer. | null |
| [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/askoptions/cancellation/) | The object for cancelling the operation. | null |

### Answering by the documents that a user may read

The language model gets only the passages of the documents that the filters of the options accept. So an application gives a user the answers only by the documents that the user may read: the passages of the other documents do not reach the model and cannot get into the answer.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);
index.AI.LanguageModel = new ChatClientLanguageModel(CreateChatClient());

AskOptions options = new AskOptions();

// Only the documents with the attribute of the department of the user
options.SearchDocumentFilter = SearchDocumentFilter.CreateAttribute("department:sales");

// The answer in the language of the user, whatever the language of the documents is
options.AdditionalInstructions = "Answer in German.";

Answer answer = index.AI.Ask("What discount do we give for an annual contract?", options);
```

## Notes

*   An answer costs one ranked search and one or two requests to the language model.
*   The method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if the language model is not set or the index does not store the text of its documents, and [ArgumentException](https://learn.microsoft.com/dotnet/api/system.argumentexception) if the question is empty. An exception of the language model, of the model of vectors or of the reranker is thrown by the method.
*   The quality of the answers depends on the language model and on the search: a model can answer only by the passages that the search has found. In an encoded index the search finds the passages by meaning, see [Search by neural terms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-neural-terms.md" >}}) and [Search by passage vectors]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-passage-vectors.md" >}}).
*   In the evaluation version an answer is made by the first 10 documents of the search, see [Evaluation limitations]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md#ai-search-limitations" >}}).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
