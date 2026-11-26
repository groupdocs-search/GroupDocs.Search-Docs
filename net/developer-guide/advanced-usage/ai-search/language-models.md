---
id: language-models
url: search/net/language-models
title: Language models
weight: 8
description: "This article describes how to connect a large language model to an index of GroupDocs.Search for .NET: the ready client of the chat completions API for OpenAI, Azure OpenAI, Ollama, LM Studio and other services, the ILanguageModel interface, and what texts the model gets."
keywords: language model, LLM, ILanguageModel, ChatCompletionLanguageModel, chat completions, OpenAI, Azure OpenAI, Ollama, LM Studio, llama.cpp, vLLM, structured output, Microsoft.Extensions.AI, privacy
productName: GroupDocs.Search for .NET
hideChildren: False
---
Four functions of the AI search use a large language model:

*   [Translating requests into queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/natural-language-queries.md" >}}): the [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) method.
*   [Answers with citations]({{< ref "search/net/developer-guide/advanced-usage/ai-search/answers-with-citations.md" >}}): the [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/) method.
*   [Sorting documents into categories]({{< ref "search/net/developer-guide/advanced-usage/ai-search/document-categories.md" >}}): the [Enrich](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/enrich/) method, for the documents that the index cannot sort confidently by itself.
*   [Suggesting synonyms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/synonym-suggestions.md" >}}): the [SuggestSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/suggestsynonyms/) method.

The library does not contain a language model and calls no service until the application tells it which one. A language model is a component of your application: a local model, a service of your company or a cloud service. The application gives it to an index as an object that implements the [ILanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ilanguagemodel/) interface, by the [LanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/languagemodel/) property of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class. The library has such an object for the services with the chat completions API, the [ChatCompletionLanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/) class; for any other model, the application implements the interface itself. The model is not kept in the index: set it after the index is opened.

These functions do not need the models of the search by meaning. The [Ask](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/ask/), [Enrich](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/enrich/) and [SuggestSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/suggestsynonyms/) methods read the text that the index [stores]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}); the [CreateQuery](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/createquery/) method works in any index.

## How the library uses a language model

The library does not let a language model act: the model gets no tools and no access to the index. It only writes a text, and the library checks what it has written.

*   Every request has instructions, a task and the required form of the reply: a JSON object described by a JSON Schema.
*   The library reads the reply and checks it against the index: the quotes of an answer are looked for in the text of the documents, a query is built and parsed by the library itself, a synonym is accepted only if it suits the rules of the index.
*   If a reply has not the required form or does not pass the check, the model is asked again with a remark. The number of the requests is limited by the MaxAttemptCount property of the options of every function: 2 by default.
*   If the model does not manage, the function tells it by its result and does not guess.
*   An application can add its own instructions by the AdditionalInstructions property of the options: for example, the language of the answers or the meaning of the fields of the documents. The form of the reply and the rules of the check are set by the library and cannot be changed by them.

## Connecting a service by the chat completions API

The [ChatCompletionLanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/) class is a language model that a service gives by the chat completions API: the API that OpenAI has introduced and that most of the services and of the programs that run models on a computer provide. The class is a part of the GroupDocs.Search assembly and uses the HTTP client of .NET, so it needs no other package.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index that stores the text of its documents
Index index = new Index(indexFolder);

// A model that a program on this computer runs: the address of the API and the name of the model
ChatCompletionLanguageModel model = new ChatCompletionLanguageModel("http://localhost:11434/v1", "model-name");

index.AI.LanguageModel = model;

Answer answer = index.AI.Ask("What is the notice period for early termination?");
Console.WriteLine(answer.Text);
```

The constructor takes the address of the API without the name of the operation, the name of the model as the service knows it, and the key of the API if the service needs one. The usual addresses are:

| Service | Address of the API | Name of the model | Key |
| --- | --- | --- | --- |
| OpenAI | `https://api.openai.com/v1` | The name of the model | The key of the API |
| Azure OpenAI | `https://YOUR-RESOURCE-NAME.openai.azure.com/openai/v1` | The name of the deployment | The key of the resource |
| Ollama | `http://localhost:11434/v1` | The name of the model | Not needed |
| LM Studio | `http://localhost:1234/v1` | The name of the model, or null for the loaded model | Not needed |
| llama.cpp (llama-server) | `http://localhost:8080/v1` | Not needed | Not needed |
| vLLM | `http://localhost:8000/v1` | The name of the model | The key that the server is started with, if any |

The ports are the default ones of the programs. Any other service or gateway with this API is connected in the same way, by its address.

**C#**

```csharp
string apiKey = Environment.GetEnvironmentVariable("OPENAI_API_KEY");
ChatCompletionLanguageModel model =
    new ChatCompletionLanguageModel("https://api.openai.com/v1", "model-name", apiKey);

// The time that a reply is waited for
model.Timeout = TimeSpan.FromMinutes(2);

// How many times a request is sent again when the service is busy
model.RetryCount = 3;

// A parameter of the service that the class does not send itself: the limit of the tokens of a reply
model.SetParameter("max_completion_tokens", "2000");

// A header that a gateway of the company requires
model.SetHeader("X-Department", "legal");
```

The members of the [ChatCompletionLanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/) class:

| Member | Description | Default |
| --- | --- | --- |
| [Timeout](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/timeout/) | The time that the reply to a request is waited for, from 1 second to 24 hours. A model on a processor writes slowly. | 10 minutes |
| [RetryCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/retrycount/) | How many times a request is sent again when the service cannot serve it at the moment, from 0 to 10. | 2 |
| [UseResponseSchema](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/useresponseschema/) | Whether the JSON Schema of the reply is sent to the service, so that a service with the structured output holds the reply in the required form. | true |
| [UseZeroTemperature](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/usezerotemperature/) | Whether the temperature 0 is sent, so that the same request gets the same reply as far as the service can do it. | true |
| [SetHeader](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/setheader/) | Sets a header of every request: the key of a service that does not take it from the Authorization header, or what a gateway requires. | |
| [SetParameter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/setparameter/) | Sets an additional member of the JSON object of every request. The value is a JSON text: a number, true or false, a string with its quotation marks, or an object. | |
| [Address](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/address/), [ModelName](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/modelname/) | The address that the requests are sent to, and the name of the model that is sent. | |
| [ServiceModelName](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/servicemodelname/) | The name of the model as the last reply of the service gives it. | null |
| [RequestCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/requestcount/), [InputTokenCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/inputtokencount/), [OutputTokenCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/outputtokencount/) | The number of the answered requests, and the numbers of the tokens that the model has read and has written, as the service counts them. | 0 |

How the class works:

*   **A request.** The instructions go as the system message, the messages of the dialogue as the messages of the user and of the assistant. The request asks for the temperature 0 and for a reply in the JSON Schema of the library (the `response_format` member of the `json_schema` type, in the strict mode), without streaming.
*   **Services that do not accept a part of a request.** Not every service and model accepts a JSON Schema or the temperature 0. A request that the service rejects with the status 400 or 422 is sent again without the rejected member, and the following requests go without it from the start: the [UseResponseSchema](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/useresponseschema/) or the [UseZeroTemperature](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/usezerotemperature/) property becomes false. A model that is not limited by a schema takes the form of the reply from the instructions, which always describe it. If you know that your service does not accept a member, set the property to false beforehand.
*   **A busy service.** A request that is answered with the status 408, 409, 429 or 500-599, or whose connection fails, is sent again after a pause: the one that the service names in the Retry-After header, if it is not longer than a minute, otherwise half a second that doubles with every attempt. A request that is not answered in the [Timeout](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/timeout/) is not sent again.
*   **Redirects.** The requests go only to the address that the application has given. If the service answers with a redirect to another address (the status 301, 302, 303, 307 or 308), the request is not sent there: the [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) exception names the other address, and it is you who decides whether to give that address to the constructor. So neither the texts nor a key of the service reach an address that you have not chosen.
*   **Cancellation.** When the operation of the index is cancelled, the request is dropped: the connection with the service is closed, so the service can stop writing the reply, and the operation returns at once. The same is done with a request that is not answered in time.
*   **Errors.** If the service does not reply in time, cannot be connected to, or replies with a status of an error, the [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) exception is thrown to the caller of the method of the index; its message has the status and the start of the reply of the service. A reply that is not a reply of this API throws the [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception) exception.
*   **Threads.** The properties, the headers and the parameters are set before the object is given to an index. The requests themselves can be sent by several threads at the same time.

The counters show what an operation has cost. Read them before and after it:

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

Index index = new Index(indexFolder);
ChatCompletionLanguageModel model = new ChatCompletionLanguageModel("http://localhost:11434/v1", "model-name");
index.AI.LanguageModel = model;

long requests = model.RequestCount;
long read = model.InputTokenCount;
long written = model.OutputTokenCount;

Answer answer = index.AI.Ask("What is the notice period for early termination?");

Console.WriteLine("Requests: " + (model.RequestCount - requests));
Console.WriteLine("Tokens that the model has read: " + (model.InputTokenCount - read));
Console.WriteLine("Tokens that the model has written: " + (model.OutputTokenCount - written));
```

A model that reasons before it replies spends more time and tokens, and these tasks do not need long reasoning. If your service has a parameter that limits the reasoning, set it by the [SetParameter](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/setparameter/) method; the name and the values of such a parameter depend on the service:

**C#**

```csharp
ChatCompletionLanguageModel model = new ChatCompletionLanguageModel("http://localhost:11434/v1", "model-name");

// The value is a JSON text, so a string is written with its quotation marks
model.SetParameter("reasoning_effort", "\"low\"");
```

Some services take the key from a header of their own, and some take a part of the settings from the parameters of the address. The parameters of the address that follow the question mark are kept:

**C#**

```csharp
// An address with a parameter: the requests go to
// https://host.example.com/openai/deployments/my-deployment/chat/completions?api-version=2024-10-21
ChatCompletionLanguageModel model = new ChatCompletionLanguageModel(
    "https://host.example.com/openai/deployments/my-deployment?api-version=2024-10-21",
    null);

// The key in a header of the service instead of the Authorization header
model.SetHeader("api-key", Environment.GetEnvironmentVariable("SERVICE_API_KEY"));
```

## Implementing the interface

An application whose model is not behind the chat completions API implements the [ILanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ilanguagemodel/) interface itself: for example, for a model that runs in the process of the application, for a service with another API, or for a client of another library. The interface has one method, [Generate](https://reference.groupdocs.com/search/net/groupdocs.search.ai/ilanguagemodel/generate/), which takes an instance of the [LanguageModelRequest](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelrequest/) class and returns an instance of the [LanguageModelResponse](https://reference.groupdocs.com/search/net/groupdocs.search.ai/languagemodelresponse/) class with the text that the model has written.

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

### A chat client of Microsoft.Extensions.AI

The clients of many services implement the `IChatClient` interface of [Microsoft.Extensions.AI](https://learn.microsoft.com/dotnet/ai/microsoft-extensions-ai). Such a client becomes a language model of an index by a small class of your application; the library itself does not depend on that package.

**C#**

```csharp
public class ChatClientLanguageModel : ILanguageModel
{
    private readonly IChatClient client;

    public ChatClientLanguageModel(IChatClient client)
    {
        this.client = client;
    }

    public LanguageModelResponse Generate(LanguageModelRequest request)
    {
        List<ChatMessage> messages = new List<ChatMessage>();
        messages.Add(new ChatMessage(ChatRole.System, request.Instructions));
        for (int i = 0; i < request.MessageCount; i++)
        {
            LanguageModelMessage message = request.GetMessage(i);
            ChatRole role = message.Role == LanguageModelRole.User ? ChatRole.User : ChatRole.Assistant;
            messages.Add(new ChatMessage(role, message.Text));
        }

        ChatOptions options = new ChatOptions();
        if (request.ResponseSchema != null)
        {
            // The JSON Schema of the reply becomes the response format of the client
            using (JsonDocument schema = JsonDocument.Parse(request.ResponseSchema))
            {
                options.ResponseFormat = ChatResponseFormat.ForJsonSchema(schema.RootElement.Clone(), "reply", null);
            }
        }

        // A chat client is asynchronous: it is called by a thread of the thread pool, and this thread waits
        ChatResponse response =
            Task.Run(() => client.GetResponseAsync(messages, options)).GetAwaiter().GetResult();
        return new LanguageModelResponse(response.Text ?? string.Empty);
    }
}
```

The class is used as any other language model:

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

Index index = new Index(indexFolder);

// A chat client that the application creates by the package of its service
IChatClient client = CreateChatClient();
index.AI.LanguageModel = new ChatClientLanguageModel(client);

Answer answer = index.AI.Ask("What is the notice period for early termination?");
Console.WriteLine(answer.Text);
```

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

To keep all the texts on your computers, use a local model: for example, a model served by Ollama or LM Studio on the same computer or in your network. The [ChatCompletionLanguageModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/chatcompletionlanguagemodel/) class sends the texts and the key of the API only to the address that the application has given to it, and it does not follow a redirect to another address; use an address of HTTPS for a service outside your network.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
