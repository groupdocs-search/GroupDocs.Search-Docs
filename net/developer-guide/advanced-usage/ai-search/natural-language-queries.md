---
id: natural-language-queries
url: search/net/natural-language-queries
title: Translating requests into queries
weight: 9
description: "This article describes how GroupDocs.Search for .NET translates a request of a user in a natural language into a query of the index by a language model: exact queries, regular expressions and the ranked search with filters."
keywords: natural language query, text to query, query translation, CreateQuery, language model, LLM, query builder, search assistant
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [query language]({{< ref "search/net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}) of GroupDocs.Search is exact and powerful, but a user of your application does not have to know it. The [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) method of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class translates a request in plain words, such as "lease agreements of 2020, only pdf", into a query of the index by a [language model]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md" >}}).

The method searches nothing. It returns the query, and the application runs it, and may show it to the user and let the user correct it before.

## How a request is translated

The language model does not write a query in the query language. It decides which search the request is for and fills a form of conditions; the library builds the query of the form, parses it by the parser of the queries of the index and checks that the index can search it. If the model names a field that the index does not have, or gives a range or a regular expression that is not valid, it is asked again with the reason.

The model gets the request, the names of the fields and of the attributes of the documents of the index, and the current date. It does not get the texts of the documents.

A request is routed to one of three searches, the values of the [QueryRoute](https://reference.groupdocs.com/search/net/groupdocs.search.ai/queryroute/) enumeration:

| Route | When | Result |
| --- | --- | --- |
| [Exact](https://reference.groupdocs.com/search/net/groupdocs.search.ai/queryroute/) | The request names what the documents must have: words, phrases, the beginning of a word, ranges of numbers and of dates, values of fields. | A query with the conditions joined by AND, OR and NOT, for the [Index.Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method. |
| [Regex](https://reference.groupdocs.com/search/net/groupdocs.search.ai/queryroute/) | The request describes the form of a word, such as a code or a number of a document. | A [regular expression]({{< ref "search/net/developer-guide/advanced-usage/searching/regular-expression-search.md" >}}) query for the [Index.Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method. |
| [Ranked](https://reference.groupdocs.com/search/net/groupdocs.search.ai/queryroute/) | The request is a question or a subject in free words. | The text for the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}); the exact conditions of the request, if it has any, become its filter. |

The extensions of the files and the [attributes]({{< ref "search/net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) that a request names become a filter of the documents.

The model does not add synonyms or forms of the words to a query: they are found by the search engine, by the options of the search of your application.

## Translating and running a request

Set the language model of the index and call the [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) method. The result is an instance of the [QueryTranslation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/) class:

| Property | Description |
| --- | --- |
| [Succeeded](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/succeeded/) | Whether the model has translated the request. |
| [Route](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/route/) | Which search the request is for. |
| [Query](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/query/) | For the Exact and Regex routes, the query to search for. For the Ranked route, the query that limits the documents of the ranked search, or null if the request has no exact conditions. |
| [QueryText](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/querytext/) | The text form of the query, in the query language of the index. |
| [RankedText](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/rankedtext/) | The text for the ranked search; null if the route is not Ranked. |
| [DocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/documentfilter/) | The filter of the documents by the extensions and the attributes that the request names, or null. |
| [Explanation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/explanation/) | A sentence of the model that tells how the request is understood, in the language of the request. |
| [FailureMessage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/failuremessage/) | The reason why the request is not translated, or null. |
| [AttemptCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/attemptcount/) | The number of the requests that the language model has got. |

The following example translates a request and runs the search that it is routed to.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);

// Setting the language model of the index
index.AI.LanguageModel = new ChatCompletionLanguageModel("http://localhost:11434/v1", "model-name");

// Translating the request of a user
QueryTranslation translation = index.AI.CreateQuery("lease agreements of 2020, only pdf");
Console.WriteLine("Understood as: " + translation.Explanation);
Console.WriteLine("Query: " + translation.QueryText);

if (translation.Route == QueryRoute.Ranked)
{
    // A question or a subject: the ranked search, limited by the exact conditions of the request
    RankedSearchOptions options = new RankedSearchOptions();
    options.Filter = translation.Query;
    options.SearchDocumentFilter = translation.DocumentFilter;
    RankedSearchResult result = index.AI.Search(translation.RankedText, options);
    Console.WriteLine("Documents: " + result.DocumentCount);
}
else
{
    // An exact query or a regular expression: the usual search
    SearchOptions options = new SearchOptions();
    options.SearchDocumentFilter = translation.DocumentFilter;
    SearchResult result = index.Search(translation.Query, options);
    Console.WriteLine("Documents: " + result.DocumentCount);
}
```

The address and the name of the model of the example are the ones of a model that a program runs on the same computer; for your service and for a model of another kind, see [Language models]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md#connecting-a-service-by-the-chat-completions-api" >}}).

The [QueryText](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/querytext/) property is what is searched for. An application can show the text to the user, let the user correct it, and run the corrected text by the [Index.Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method that takes a query in text form.

## When a request is not translated

A request of a user is never lost. If the language model has not given a query that can be searched in the attempts that it has, or if the operation is cancelled:

*   the [Succeeded](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/succeeded/) property is false, and the [FailureMessage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/failuremessage/) property tells the reason;
*   the [Route](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/route/) property is [Ranked](https://reference.groupdocs.com/search/net/groupdocs.search.ai/queryroute/), and the [RankedText](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/rankedtext/) property is the request itself.

So the code of the example above works in this case too: the request goes to the ranked search as it is.

## Options of the translation

The [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) method has an overload that takes an instance of the [QueryTranslationOptions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslationoptions/) class:

| Property | Description | Default |
| --- | --- | --- |
| [MaxAttemptCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslationoptions/maxattemptcount/) | The greatest number of the requests to the language model for one translation. | 2 |
| [CurrentDate](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslationoptions/currentdate/) | The date that the model takes for today when a request names a date relative to it, such as "last year". | The current date of the system |
| [AdditionalInstructions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslationoptions/additionalinstructions/) | The instructions of the application that are added to the instructions of the library: for example, what the fields of the documents mean. | null |
| [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslationoptions/cancellation/) | The object for cancelling the operation. | null |

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);
index.AI.LanguageModel = new ChatCompletionLanguageModel("http://localhost:11434/v1", "model-name");

QueryTranslationOptions options = new QueryTranslationOptions();
options.AdditionalInstructions =
    "The attribute 'confidential' marks the documents of the legal department. " +
    "The field 'author' has the name of the person who has written a document.";

QueryTranslation translation = index.AI.CreateQuery("confidential documents written by Smith last year", options);
```

## Notes

*   The method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if the language model is not set, and [ArgumentException](https://learn.microsoft.com/dotnet/api/system.argumentexception) if the request is empty. An exception of the language model is thrown by the method.
*   A query that the index cannot search is not returned. For example, a date range cannot be searched in a compact index: the model is asked again with this reason, and if it gives no other query, the request goes to the ranked search as it is.
*   The quality of the translation depends on the language model. The library guarantees that the returned query is parsed by the index and can be searched in it; it cannot guarantee that the model has understood the request as the user meant. Show the [Explanation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/explanation/) and the [QueryText](https://reference.groupdocs.com/search/net/groupdocs.search.ai/querytranslation/querytext/) properties to the user where it matters.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
