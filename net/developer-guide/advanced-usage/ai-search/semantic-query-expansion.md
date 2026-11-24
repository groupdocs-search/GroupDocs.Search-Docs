---
id: semantic-query-expansion
url: search/net/semantic-query-expansion
title: Semantic expansion of queries
weight: 7
description: "This article describes the semantic expansion of queries in GroupDocs.Search for .NET: an option of the usual search that finds the words of the index close in meaning to the words of a query, such as word forms, synonyms and translations, by a word vectors model."
keywords: semantic expansion, query expansion, word vectors, word embeddings, synonyms without dictionary, cross-language search, SemanticExpansionOptions
productName: GroupDocs.Search for .NET
hideChildren: False
---
The semantic expansion is an option of the usual search of the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method. With it, a word of a query is searched together with the words of the index that are close to it in meaning: the forms of the word, its synonyms and its translations. For example, with the tested model, the word "contract" also finds "contracts" and, in an index of documents in several languages, the Russian word "контракт".

The close words are found by a word vectors model, which keeps a vector of meaning for every word of its vocabulary. No dictionary has to be filled, no neural network is run and the documents do not have to be encoded. Everything else in the usual search works as before: the operators of the query language, the phrases and the fields, the highlighting and the occurrences.

## Enabling the expansion

1.  Add a [model package]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md" >}}) of the [WordVectors](https://reference.groupdocs.com/search/net/groupdocs.search.ai/modelrole/) role to the index by the [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method. The model is not kept in the index: add it every time the index is opened. The call reads the vectors into memory, which takes about a second for the tested model of 55 MB.
2.  Set the [Enabled](https://reference.groupdocs.com/search/net/groupdocs.search.options/semanticexpansionoptions/enabled/) property of the [SemanticExpansion](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/semanticexpansion/) options of a search.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string modelFolder = @"c:\Models\static-similarity-mrl-multilingual-v1";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Adding the word vectors model: no runtime is needed
index.AI.AddModel(ModelPackage.Load(modelFolder));

// Enabling the semantic expansion
SearchOptions options = new SearchOptions();
options.SemanticExpansion.Enabled = true;

// In addition to the word "contract", the words close to it in meaning are found, for example "contracts"
SearchResult result = index.Search("contract", options);

// Printing the found words of the first document
if (result.DocumentCount > 0)
{
    FoundDocument document = result.GetFoundDocument(0);
    Console.WriteLine(document.DocumentInfo.FilePath);
    Console.WriteLine("    " + string.Join(", ", document.Terms));
}
```

A search with the option enabled in an index without a word vectors model is performed without the expansion.

## Options of the expansion

The [SemanticExpansionOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/semanticexpansionoptions/) class has the following properties:

| Property | Description | Default |
| --- | --- | --- |
| [Enabled](https://reference.groupdocs.com/search/net/groupdocs.search.options/semanticexpansionoptions/enabled/) | Whether the words of a query are expanded. | false |
| [MinSimilarity](https://reference.groupdocs.com/search/net/groupdocs.search.options/semanticexpansionoptions/minsimilarity/) | The least similarity of a word of the index to a word of the query with which the word of the index is searched: the cosine of the angle between the vectors of the two words, greater than 0 and not greater than 1. | 0.7 |
| [MaxExpansionCount](https://reference.groupdocs.com/search/net/groupdocs.search.options/semanticexpansionoptions/maxexpansioncount/) | The greatest number of the terms of the index that a word of the query is expanded into, from 1 to 100. The most similar terms are taken. | 10 |

With the tested model, the forms of a word and its closest translations usually have a similarity of 0.8 and more, and its synonyms from 0.6 to 0.8. The words of a smaller similarity are mostly words of the same subject, not of the same meaning. Raise the [MinSimilarity](https://reference.groupdocs.com/search/net/groupdocs.search.options/semanticexpansionoptions/minsimilarity/) property to make a search more exact, and lower it to find more.

## How the words are expanded

*   A word of a query is expanded if the model knows it as a whole word: one token of its vocabulary. The tested model knows about 55,000 whole words of many languages. A word that the model divides into parts is searched as it is: the mean of the vectors of the parts describes a word badly.
*   A word is expanded into the terms of the index that the model also knows as whole words. Only the words that the documents of the index really have are searched.
*   The model keeps the words in the lower case and without accents, so a word of the model stands for the terms of the index with any variants of its letters: "lease" also finds "léase".
*   The terms that a word of the query is already searched by, such as its [synonyms]({{< ref "search/net/developer-guide/advanced-usage/searching/synonym-search.md" >}}), its [word forms]({{< ref "search/net/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}) and the variants of the [fuzzy search]({{< ref "search/net/developer-guide/advanced-usage/searching/fuzzy-search.md" >}}), are not taken again and do not count in the [MaxExpansionCount](https://reference.groupdocs.com/search/net/groupdocs.search.options/semanticexpansionoptions/maxexpansioncount/) limit. The found terms are not expanded further.
*   The found words are treated as the synonyms of the word: they are [highlighted]({{< ref "search/net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}), counted as the occurrences of the word and, in the [order by relevance]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md#resultorder-property" >}}), weighed as the word itself.
*   The close words of a word of a query are found in about 15 milliseconds and are remembered, so a repeated word costs nothing.

The words that a word of a query is expanded into are reported by the [SearchPhaseCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/searchphasecompleted/) event of the index with the [SearchPhase.SemanticExpansion](https://reference.groupdocs.com/search/net/groupdocs.search.events/searchphase/) phase, after the phase of the fuzzy search.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string modelFolder = @"c:\Models\static-similarity-mrl-multilingual-v1";

// Opening an index
Index index = new Index(indexFolder);
index.AI.AddModel(ModelPackage.Load(modelFolder));

// Subscribing to the event of the search phases
index.Events.SearchPhaseCompleted += (sender, args) =>
{
    if (args.SearchPhase == SearchPhase.SemanticExpansion)
    {
        Console.WriteLine(string.Join(", ", args.Words));
    }
};

SearchOptions options = new SearchOptions();
options.SemanticExpansion.Enabled = true;
options.SemanticExpansion.MinSimilarity = 0.8; // Only the closest words
options.SemanticExpansion.MaxExpansionCount = 5;
SearchResult result = index.Search("contract", options);
```

## When to use the expansion

The expansion widens a search: it finds more documents, and some of them by words that only resemble the word of the query. It is a means of the recall of the usual search, when a user wants all the documents on a subject and reviews the occurrences.

It is not a means of ordering. To get the most relevant documents first, also for a query in another language, use the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}) with the [passage vectors]({{< ref "search/net/developer-guide/advanced-usage/ai-search/search-by-passage-vectors.md" >}}), which the same model package serves.

| | Synonym search | Semantic expansion |
| --- | --- | --- |
| Source of the words | The [synonym dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/synonym-dictionary.md" >}}) of the index, which you fill and control. | A word vectors model. |
| Languages | The languages of the dictionary; the default dictionary is English. | The languages of the model, and between them. |
| Exactness | Exactly the words of the dictionary. | The words of a similarity above the threshold; some of them are only related. |

Both options can be enabled in one search. The [suggestion of synonyms]({{< ref "search/net/developer-guide/advanced-usage/ai-search/synonym-suggestions.md" >}}) helps to fill the dictionary.

## Limitations

*   The semantic expansion cannot be combined with the [case-sensitive search]({{< ref "search/net/developer-guide/advanced-usage/searching/case-sensitive-search.md" >}}). Such a search returns an empty result and reports the error, see [Incompatible options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}}).
*   The [search network]({{< ref "search/net/developer-guide/advanced-usage/scaling/_index.md" >}}) does not use the option.
*   The ranked search of the [IndexAI.Search](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/search/) method does not use the option.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
