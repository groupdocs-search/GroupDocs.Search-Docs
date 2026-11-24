---
id: synonym-suggestions
url: search/net/synonym-suggestions
title: Suggesting synonyms
weight: 12
description: "This article describes how GroupDocs.Search for .NET suggests synonyms for the synonym dictionary of an index: a language model names the synonyms of the words of the index, the index checks them, and a person reviews the suggestions."
keywords: synonym suggestions, synonym dictionary, SuggestSynonyms, GetDistinctiveWords, language model, LLM, synonym search, keywords of documents
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [synonym search]({{< ref "search/net/developer-guide/advanced-usage/searching/synonym-search.md" >}}) finds the synonyms of the words of a query that the [synonym dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/synonym-dictionary.md" >}}) of the index has. The default dictionary has common English synonyms; the words of your documents, such as the terms of your business, and the words of other languages have to be added to it. The [SuggestSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/suggestsynonyms/) method of the [IndexAI](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/) class helps to fill the dictionary:

1.  A [language model]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md" >}}) names the synonyms of the words of the index.
2.  The index checks what is named and counts the documents that have each word.
3.  A person reviews the suggestions, and the accepted ones are added to the dictionary.

The method changes nothing by itself: neither the dictionary nor the index. The model gets words only, never the texts of the documents.

## Getting the suggestions

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index that stores the text of its documents
Index index = new Index(indexFolder);

// Setting the language model of the index
index.AI.LanguageModel = new ChatClientLanguageModel(CreateChatClient());

// A few words about the collection help the model to choose the meaning of a word
SynonymSuggestionOptions options = new SynonymSuggestionOptions();
options.AdditionalInstructions = "The documents are lease contracts in English.";

// The index chooses the words to examine and asks the model about them
SynonymSuggestions suggestions = index.AI.SuggestSynonyms(options);

// Printing the suggestions with the numbers of the documents that have the words
foreach (ExaminedWord word in suggestions)
{
    for (int i = 0; i < word.SynonymCount; i++)
    {
        SuggestedSynonym synonym = word.GetSynonym(i);
        Console.WriteLine(
            word.Word + " (" + word.DocumentCount + ") - " +
            synonym.Word + " (" + synonym.DocumentCount + ")" +
            (synonym.IsMutual ? ", mutual" : string.Empty));
    }
}
```

The `CreateChatClient` method of the example stands for the code that creates a chat client of your service, see [Language models]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md#connecting-a-model-by-microsoftextensionsai" >}}).

The result is an instance of the [SynonymSuggestions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestions/) class: the examined words, each an instance of the [ExaminedWord](https://reference.groupdocs.com/search/net/groupdocs.search.ai/examinedword/) class with the synonyms that are suggested for it, instances of the [SuggestedSynonym](https://reference.groupdocs.com/search/net/groupdocs.search.ai/suggestedsynonym/) class.

| Member | Description |
| --- | --- |
| [ExaminedWord.Word](https://reference.groupdocs.com/search/net/groupdocs.search.ai/examinedword/word/) | The examined word, in the lower case. |
| [ExaminedWord.DocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/examinedword/documentcount/) | The number of the documents of the index that have the word. |
| [ExaminedWord.SynonymCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/examinedword/synonymcount/), [GetSynonym](https://reference.groupdocs.com/search/net/groupdocs.search.ai/examinedword/getsynonym/) | The synonyms that are suggested for the word, the closest first. |
| [ExaminedWord.FailureMessage](https://reference.groupdocs.com/search/net/groupdocs.search.ai/examinedword/failuremessage/) | The reason why the model has given no reply about the word, or null. |
| [SuggestedSynonym.Word](https://reference.groupdocs.com/search/net/groupdocs.search.ai/suggestedsynonym/word/) | The suggested synonym. |
| [SuggestedSynonym.DocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/suggestedsynonym/documentcount/) | The number of the documents of the index that have the synonym. |
| [SuggestedSynonym.IsMutual](https://reference.groupdocs.com/search/net/groupdocs.search.ai/suggestedsynonym/ismutual/) | Whether the model has also named the word as a synonym of the synonym, when it was asked about the synonym separately. Two agreeing replies weigh more than one. |

## Which words are examined

### Words of the application

The best source of the words is the queries of the users of your application: these are the words that people really search for. Give them by the [Words](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/words/) property. A text of the array is divided into words by the rules of the index, so it may be a whole query; every word is examined once, in the lower case.

A word does not have to be in the index. A word that the users search for and the documents do not have is exactly the word whose synonym in the documents is worth knowing.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);
index.AI.LanguageModel = new ChatClientLanguageModel(CreateChatClient());

SynonymSuggestionOptions options = new SynonymSuggestionOptions();
options.Words = new string[] { "landlord", "early termination", "apartment rent" };
options.IndexedSynonymsOnly = true; // Only the synonyms that the documents of the index have

SynonymSuggestions suggestions = index.AI.SuggestSynonyms(options);
```

### Words that the index chooses

If the words are not given, the index chooses them itself: the words that tell the documents of the index apart, that is, the words of the subjects of the documents rather than the words of every text. The same words are returned by the [GetDistinctiveWords](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/getdistinctivewords/) method, which uses no model at all. An application can get the words, look at them, leave some of them out, and ask the model about them in parts.

The words are chosen by the stored text of a part of the documents: the beginning of the text of up to 1000 documents spread evenly over the index. A word is taken if it has three characters or more, has a letter and no digits, is not a stop word of an index that uses the stop words, and is found in at least one document of every fifty.

Names of people and of companies are often among the chosen words: statistics cannot tell them from the words of a subject. The model is told to give no synonyms for a name, so they only take places among the examined words. Remove them from the list to save requests.

## What the index checks

The index does not take the word of the model for it. A suggestion that cannot work does not reach the reviewer:

*   A synonym must be exactly one word by the rules of the index, because the synonym dictionary replaces a word of a query by a word.
*   The word itself and a repeated synonym are dropped.
*   A pair that the synonym dictionary of the index already has is not suggested.
*   One of the two words of a pair must be in the documents of the index; otherwise the pair would find nothing. By default, a synonym that no document has is suggested if the documents have the examined word: such a synonym serves the queries. With the [IndexedSynonymsOnly](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/indexedsynonymsonly/) option, only the synonyms that the documents have are suggested.

The index cannot check the meaning. The model names words of close meaning, and some of them are not synonyms in the documents of your index; it may also name a form of the word instead of a synonym. This is why the suggestions are for a person to review.

## Adding the accepted synonyms to the dictionary

The [ExportSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestions/exportsynonyms/) method writes the suggested pairs to a text file in the format that the synonym dictionary imports. A person edits the file in a text editor: deletes the pairs that are wrong, and the rest is imported.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string reviewFile = @"c:\Review\Synonyms.txt";

// Opening an index
Index index = new Index(indexFolder);
index.AI.LanguageModel = new ChatClientLanguageModel(CreateChatClient());

// Writing the suggestions to a file for a review
SynonymSuggestions suggestions = index.AI.SuggestSynonyms();
suggestions.ExportSynonyms(reviewFile);

// After the review: adding the pairs of the file to the synonym dictionary of the index
index.Dictionaries.SynonymDictionary.ImportDictionary(reviewFile);

// The synonym search uses the new pairs
SearchOptions searchOptions = new SearchOptions();
searchOptions.UseSynonymSearch = true;
SearchResult result = index.Search("landlord", searchOptions);
```

An application can also show the suggestions in its own interface and add the accepted pairs by the [AddRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/synonymdictionary/addrange/) method of the dictionary; the [CreateSynonymGroups](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestions/createsynonymgroups/) method returns all the suggested pairs as arrays for that method.

The suggestions are pairs, not groups: a group of three words in the dictionary makes all three synonyms of each other, and the synonyms of the different meanings of a word are not synonyms of each other.

To take a careful part of the suggestions without a review, take the mutual pairs whose both words are in the documents.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);
index.AI.LanguageModel = new ChatClientLanguageModel(CreateChatClient());

SynonymSuggestions suggestions = index.AI.SuggestSynonyms();

// Taking the pairs that the model has named from both sides and that the documents have
List<string[]> accepted = new List<string[]>();
foreach (ExaminedWord word in suggestions)
{
    for (int i = 0; i < word.SynonymCount; i++)
    {
        SuggestedSynonym synonym = word.GetSynonym(i);
        if (synonym.IsMutual && word.DocumentCount > 0 && synonym.DocumentCount > 0)
        {
            accepted.Add(new string[] { word.Word, synonym.Word });
        }
    }
}

index.Dictionaries.SynonymDictionary.AddRange(accepted);
```

## Working in parts

A long work is better done in parts: an exception of the language model, for example when its service is not available, is thrown by the [SuggestSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/suggestsynonyms/) method, and what the call has found is lost.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Opening an index
Index index = new Index(indexFolder);
index.AI.LanguageModel = new ChatClientLanguageModel(CreateChatClient());

// Getting the words without a model: the list can be reviewed and shortened
string[] words = index.AI.GetDistinctiveWords(2000);

// Asking the model about 200 words at a time
for (int start = 0; start < words.Length; start += 200)
{
    int count = Math.Min(200, words.Length - start);
    string[] part = new string[count];
    Array.Copy(words, start, part, 0, count);

    SynonymSuggestionOptions options = new SynonymSuggestionOptions();
    options.Words = part;
    SynonymSuggestions suggestions = index.AI.SuggestSynonyms(options);
    suggestions.ExportSynonyms(@"c:\Review\Synonyms" + (start / 200 + 1) + ".txt");
}
```

## Options

| Property | Description | Default |
| --- | --- | --- |
| [Words](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/words/) | The words to examine, for example the words of the queries of the users. | null: the index chooses the words |
| [MaxWordCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/maxwordcount/) | The greatest number of the words that are examined, from 1 to 100,000. | 200 |
| [MaxWordsPerRequest](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/maxwordsperrequest/) | The greatest number of the words of one request to the language model. More words in a request mean fewer requests and a longer reply to each: a small model follows the required form of a long reply worse. | 20 |
| [MaxSynonymsPerWord](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/maxsynonymsperword/) | The greatest number of the synonyms that are suggested for one word, from 1 to 20. | 5 |
| [IndexedSynonymsOnly](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/indexedsynonymsonly/) | Whether only the synonyms that the documents of the index have are suggested. | false |
| [MaxAttemptCount](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/maxattemptcount/) | The greatest number of the requests to the language model for one group of words. | 2 |
| [AdditionalInstructions](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/additionalinstructions/) | The instructions that are added to the instructions of the language model: for example, what the documents are about and in which language. | null |
| [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/cancellation/) | The object for cancelling the operation. A cancelled operation returns the words that it has examined. | null |

## Notes

*   The operation reads the index as a search does: the index can be searched and changed while the language model works.
*   The model sees the words without their texts, so it chooses the meaning of a word that has several by the other words of the request and by the additional instructions. A few words about the collection help noticeably.
*   The words that the index chooses depend on the beginning of the text of the documents. Give a word that is found only further in the texts by the [Words](https://reference.groupdocs.com/search/net/groupdocs.search.ai/synonymsuggestionoptions/words/) property.
*   The method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if the language model is not set, or if the words are not given and the index does not store the text of its documents.
*   The [semantic expansion of queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/semantic-query-expansion.md" >}}) finds the words of close meaning without a dictionary, by a word vectors model. The two means complement each other: the dictionary is exact and under your control, and the expansion needs no work.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
