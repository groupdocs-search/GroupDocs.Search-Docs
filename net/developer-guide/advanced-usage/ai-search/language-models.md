---
id: language-models
url: search/net/language-models
title: Language models
weight: 8
description: "This article describes how to connect a large language model to an index of GroupDocs.Search for .NET: the ILanguageModel interface, the bridge to Microsoft.Extensions.AI chat clients such as OpenAI, Azure OpenAI and Ollama, and what texts the model gets."
keywords: language model, LLM, ILanguageModel, ChatClientLanguageModel, Microsoft.Extensions.AI, IChatClient, OpenAI, Ollama, LM Studio, structured output, privacy
productName: GroupDocs.Search for .NET
hideChildren: False
---
Four functions of the AI search use a large language model:

*   [Translating requests into queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/natural-language-queries.md" >}}): the [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) method.
*   [Answers with citations]({{< ref "search/net/developer-guide/advanced-usage/ai-search/answers-with-citations.md" >}}): the [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/) method.
*   [Sorting documents into categories]({{< ref "search/net/developer-guide/advanced-usage/ai-search/document-categories.md" >}}): the [Enrich](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/enrich/) method, for the documents that the index cannot sort confidently by itself.
*   [Suggesting synonyms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/synonym-suggestions.md" >}}): the [SuggestSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/suggestsynonyms/) method.

The library does not contain a language model and does not call any service by itself. A language model is a component of your application: a local model, a service of your company or a cloud service. The application gives it to an index as an object that implements the [ILanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ilanguagemodel/) interface, by the [LanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/languagemodel/) property of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class. The model is not kept in the index: set it after the index is opened.

These functions do not need the models of the search by meaning. The [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/), [Enrich](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/enrich/) and [SuggestSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/suggestsynonyms/) methods read the text that the index [stores]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}); the [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) method works in any index.

## How the library uses a language model

The library does not let a language model act: the model gets no tools and no access to the index. It only writes a text, and the library checks what it has written.

*   Every request has instructions, a task and the required form of the reply: a JSON object described by a JSON Schema.
*   The library reads the reply and checks it against the index: the quotes of an answer are looked for in the text of the documents, a query is built and parsed by the library itself, a synonym is accepted only if it suits the rules of the index.
*   If a reply has not the required form or does not pass the check, the model is asked again with a remark. The number of the requests is limited by the MaxAttemptCount property of the options of every function: 2 by default.
*   If the model does not manage, the function tells it by its result and does not guess.
*   An application can add its own instructions by the AdditionalInstructions property of the options: for example, the language of the answers or the meaning of the fields of the documents. The form of the reply and the rules of the check are set by the library and cannot be changed by them.

## Connecting a model by Microsoft.Extensions.AI

The GroupDocs.Search.AI.Connectors package has a ready implementation of the interface, the [ChatClientLanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai.connectors/chatclientlanguagemodel/) class of the `GroupDocs.Search.AI.Connectors` namespace. It makes a language model of an index of any chat client of [Microsoft.Extensions.AI](https://learn.microsoft.com/dotnet/ai/microsoft-extensions-ai): the `IChatClient` interface, which is implemented by the clients of OpenAI, Azure OpenAI, Ollama and other services, and by any service with an OpenAI-compatible API, such as LM Studio and vLLM.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index that stores the text of its documents
Index index = new Index(indexFolder);

// A chat client of Microsoft.Extensions.AI that the application creates by the package of its service
IChatClient client = CreateChatClient();

// The language model of the index over the client
ChatClientLanguageModel model = new ChatClientLanguageModel(client);

// The options that every request of the index is sent with
ChatOptions chatOptions = new ChatOptions();
chatOptions.Temperature = 0;
chatOptions.MaxOutputTokens = 2000;
model.Options = chatOptions;

index.AI.LanguageModel = model;

Answer answer = index.AI.Ask("What is the notice period for early termination?");
Console.WriteLine(answer.Text);
```

The client is created by the package of the service: the `CreateChatClient` method of the example stands for the code of your application. With the Microsoft.Extensions.AI.OpenAI package, for OpenAI or a service with an OpenAI-compatible API, it is:

**C#**

```csharp
OpenAIClientOptions clientOptions = new OpenAIClientOptions();
clientOptions.Endpoint = new Uri("http://localhost:1234/v1"); // The address of the service
IChatClient client =
    new OpenAI.Chat.ChatClient("model-name", new ApiKeyCredential("key"), clientOptions).AsIChatClient();
```

With Ollama and the OllamaSharp package, it is:

**C#**

```csharp
IChatClient client = new OllamaApiClient(new Uri("http://localhost:11434/"), "model-name");
```

The [ChatClientLanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai.connectors/chatclientlanguagemodel/) class has two properties:

| Property | Description | Default |
| --- | --- | --- |
| [Options](https://reference.groupdocs.com/search/net/groupdocs.search.ai.connectors/chatclientlanguagemodel/options/) | The options that every request is sent with: the model, the temperature, the limit of the tokens of a reply, the additional properties that the client reads. Every request gets a copy of the object. | null |
| [UseResponseSchema](https://reference.groupdocs.com/search/net/groupdocs.search.ai.connectors/chatclientlanguagemodel/useresponseschema/) | Whether the client is given the JSON Schema of the reply as the response format. A service with the structured output then holds the reply in the required form. Set false for a client or a service that does not accept a schema: the model takes the form of the reply from the instructions, which always describe it. | true |

How the class works:

*   It adds nothing to a request by itself. In particular, it does not set the temperature: the replies repeat best at the temperature 0, but some models accept only their own temperature and refuse a request with another one. Set what your model accepts by the [Options](https://reference.groupdocs.com/search/net/groupdocs.search.ai.connectors/chatclientlanguagemodel/options/) property.
*   The methods of an index are synchronous, and a chat client is asynchronous. The client is called by a thread of the thread pool while the calling thread waits, so a client does not hang in an application with a synchronization context, such as a desktop application.
*   When the operation of the index is cancelled, the call of the client is cancelled by its cancellation token, and the operation returns at once.
*   An exception of the client is thrown to the caller of the method of the index as it is.
*   The class does not own the client and does not dispose it.

## Implementing the interface

An application that cannot use Microsoft.Extensions.AI implements the [ILanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ilanguagemodel/) interface itself. The interface has one method, [Generate](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ilanguagemodel/generate/), which takes an instance of the [LanguageModelRequest](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelrequest/) class and returns an instance of the [LanguageModelResponse](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelresponse/) class with the text that the model has written.

| Member of the request | Description | What a chat service gets |
| --- | --- | --- |
| [Instructions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelrequest/instructions/) | What the model must do and how it must reply. | The system message. |
| [MessageCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelrequest/messagecount/), [GetMessage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelrequest/getmessage/) | The messages of the dialogue in their order. The first message is the task; a reply of the model that the index has not accepted and the remark of the index on it may follow. | The messages of the user ([LanguageModelRole.User](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelrole/)) and of the assistant ([LanguageModelRole.Model](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelrole/)). |
| [ResponseSchema](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelrequest/responseschema/) | The JSON Schema of the JSON object that the model must reply with, or null if the reply is a text of any form. | The response format, if the service can limit a reply by a schema. |
| [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelrequest/cancellation/) | The object that tells that the reply is not needed any more. It is never null. | The implementation may stop waiting for the service and return any reply when the [IsCancelled](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/iscancelled/) property becomes true. |

**C#**

```csharp
public class MyLanguageModel : ILanguageModel
{
    public LanguageModelResponse Generate(LanguageModelRequest request)
    {
        // The roles and the texts of the messages of a chat
        List<KeyValuePair<string, string>> messages = new List<KeyValuePair<string, string>>();
        messages.Add(new KeyValuePair<string, string>("system", request.Instructions));
        for (int i = 0; i < request.MessageCount; i++)
        {
            LanguageModelMessage message = request.GetMessage(i);
            string role = message.Role == LanguageModelRole.User ? "user" : "assistant";
            messages.Add(new KeyValuePair<string, string>(role, message.Text));
        }

        // The JSON Schema of the reply, or null
        string schema = request.ResponseSchema;

        string reply = Send(messages, schema, request.Cancellation);
        return new LanguageModelResponse(reply);
    }

    private string Send(List<KeyValuePair<string, string>> messages, string schema, Cancellation cancellation)
    {
        // Call your model or service here and return the text of its reply
        throw new NotImplementedException();
    }
}
```

Requirements to an implementation:

*   The [Generate](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ilanguagemodel/generate/) method can be called by several threads at the same time.
*   An exception that the method throws stops the operation of the index and is thrown to the caller of the operation.
*   The reply is the text of the model as it is. The library finds the JSON object in the reply itself and removes the reasoning that some models write in `<think>` tags before the reply.

## Choosing a model

*   Any chat model that follows instructions and replies with JSON suits. A model or a service with the structured output, which limits a reply by a JSON Schema, holds the form of the reply best.
*   The size of a request is moderate. The [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/) method sends 8 passages of about a thousand characters each by default; the [Enrich](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/enrich/) method sends the first 4000 characters of a document by default; the [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) and [SuggestSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/suggestsynonyms/) methods send short lists of names and words. A model with a small context window is enough.
*   A weaker model makes more mistakes of form and is asked again more often. The results stay checked: a reply that does not pass the check is not accepted from any model.
*   The quality of the answers, of the queries and of the suggested synonyms depends on the model. Try the functions with your model on your documents before you rely on them.

## Privacy

If the language model is a remote service, the texts that the library gives to it leave the computer. The library gives texts only to the object that the application has set by the [LanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/languagemodel/) property, and each function gives only what it needs:

| Function | What the model gets |
| --- | --- |
| [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) | The request of the user, the names of the fields and of the attributes of the documents of the index, and the current date. No texts of the documents. |
| [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/) | The question, and the best passages that the search has found for it, with the names of their files without folders. The filters of the options limit the documents. |
| [Enrich](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/enrich/) | The descriptions of the categories, the name of a document and the beginning of its text. Only if the options ask for the language model, and only for the documents that the index has not sorted itself. |
| [SuggestSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/suggestsynonyms/) | Words of the index, or the words that the application gives. No texts of the documents. |

To keep all the texts on your computers, use a local model: for example, a model served by Ollama or LM Studio on the same computer or in your network.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
