---
id: search-options
url: search/net/search-options
title: Search options
weight: 22
description: "This article describes the search options that can be specified in an instance of the SearchOptions class."
keywords: Search options
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains a description of all search options that can be specified in an instance of the [SearchOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/) class. If no search options are passed to the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method, the default values described below are used.

## Cancellation property

The [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/cancellation/) property allows you to specify the object to cancel the search operation. The search can be canceled without delay by calling the [Cancel](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/cancel/) method or by calling the [CancelAfter](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/cancelafter/) method with the delay in milliseconds as an argument. The default value is null, which means that the search cannot be canceled. To cancel the continuation of a search by chunks, pass the cancellation object to the [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search/index/searchnext/) method (see the [Search by chunks]({{< ref "search/net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) page).

Since version 26.11, the [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/) class implements IDisposable. A cancellation scheduled by the [CancelAfter](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/cancelafter/) method keeps a timer until it is requested: dispose of the cancellation object, for example by a using statement, to release the timer. The scheduled cancellation is then not requested; a cancellation that has been requested already stays requested. After the disposal, the CancelAfter method throws ObjectDisposedException.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string query = "Einstein";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Creating a cancellation object that is disposed of after the search
using (Cancellation cancellation = new Cancellation())
{
    // The search is canceled if it takes longer than 10 seconds
    cancellation.CancelAfter(10000);

    // Creating a search options instance
    SearchOptions options = new SearchOptions();
    options.Cancellation = cancellation;

    // Search in the index
    SearchResult result = index.Search(query, options);
    Console.WriteLine("Document count: " + result.DocumentCount);
}
// The cancellation object is disposed of: its timer is released, and the cancellation is not requested later
```

## DateFormats property

The [DateFormats](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/dateformats/) property allows you to specify formats for the date range search feature. By default, the collection contains the formats 'dd.MM.yyyy', 'MM/dd/yyyy', and 'yyyy-MM-dd'. For details, see the [Date range search]({{< ref "search/net/developer-guide/advanced-usage/searching/date-range-search.md" >}}) page.

## FuzzySearch property

The [FuzzySearch](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/fuzzysearch/) property returns a fuzzy search options object that allows you to enable fuzzy search and set its options. By default, the fuzzy search feature is disabled. For details, see the [Fuzzy search]({{< ref "search/net/developer-guide/advanced-usage/searching/fuzzy-search.md" >}}) page.

## IsChunkSearch property

The [IsChunkSearch](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/ischunksearch/) property allows you to enable search by chunks. By default, the search by chunks feature is disabled. Since version 26.11, the search by chunks is also available in an index repository. For details, see the [Search by chunks]({{< ref "search/net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) page.

## KeyboardLayoutCorrector property

The [KeyboardLayoutCorrector](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/keyboardlayoutcorrector/) property returns a keyboard layout corrector options object, which allows you to enable keyboard layout correction in search queries. By default, the keyboard layout correction feature is disabled. See the [Keyboard layout correction]({{< ref "search/net/developer-guide/advanced-usage/searching/keyboard-layout-correction.md" >}}) page for details.

## MaxOccurrenceCountPerTerm property

The [MaxOccurrenceCountPerTerm](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/maxoccurrencecountperterm/) property allows you to specify the maximum number of occurrences of each word found that will be returned as a search result. The default value is 2000000000. Since version 26.11, a value less than 1 throws ArgumentOutOfRangeException; before, a search with such a value found nothing.

## MaxTotalOccurrenceCount property

The [MaxTotalOccurrenceCount](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/maxtotaloccurrencecount/) property allows you to specify the maximum number of occurrences in total for all found words, which will be returned as a search result. The default value is 2000000000. Since version 26.11, a value less than 1 throws ArgumentOutOfRangeException; before, a search with such a value found nothing.

When a limit of the occurrences is reached, the result is truncated: its [Truncated](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/truncated/) property is true, and its [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) property names the part of the query whose occurrences are discarded. Since version 26.11, a search that reaches a limit stops reading the index:

*   The documents after the limit are not found. Before, the search went on and found such documents without occurrences.
*   The limit is reported by one line of the warnings, or by one line for each chunk of a search by chunks. Before, the warnings had a line for every word and index segment that the limit stopped.
*   The result is truncated also when the limit lets a part of the query search nothing, and when the query excluded with NOT is truncated.

## ResultOrder property

The [ResultOrder](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/resultorder/) property allows you to specify the order of the found documents in the search result. The property is available since version 26.12. The default value is [SearchResultOrder.Default](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchresultorder/): the documents go in the order of the index, as in the previous versions.

With the [SearchResultOrder.Bm25](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchresultorder/) value, the found documents are ordered by relevance, the most relevant first, and the [Relevance](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/relevance/) property of a found document returns its [Okapi BM25](https://en.wikipedia.org/wiki/Okapi_BM25) score. The documents of equal relevance go in the order of the index. The search finds the same documents and the same occurrences as with the default order:

*   The score is counted by the found terms and phrases: a rare word counts for more than a frequent one, and many occurrences in a short document count for more than the same occurrences in a long one.
*   The terms that a word of the query is expanded to, such as its forms, its synonyms and the terms found by the fuzzy search, are weighed as the word itself.
*   If the [MaxTotalOccurrenceCount](#maxtotaloccurrencecount-property) and [MaxOccurrenceCountPerTerm](#maxoccurrencecountperterm-property) limits cut the result, the score is counted by the occurrences that are found.
*   In a [search by chunks]({{< ref "search/net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}), the documents of every chunk are ordered, and the scores are counted by the documents of the chunk.
*   A search of an [index repository]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-repository.md" >}}) orders the documents of all its indexes together, by the scores counted in each index.
*   The [search network]({{< ref "search/net/developer-guide/advanced-usage/scaling/_index.md" >}}) does not use the option.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Creating a search options instance
SearchOptions options = new SearchOptions();
options.ResultOrder = SearchResultOrder.Bm25; // Ordering the found documents by relevance

// Searching for the documents that have any of the two words: the most relevant documents go first
SearchResult result = index.Search("lease OR termination", options);
for (int i = 0; i < result.DocumentCount; i++)
{
    FoundDocument document = result.GetFoundDocument(i);
    Console.WriteLine(document.Relevance.ToString("F3") + "  " + document.DocumentInfo.FilePath);
}
```

To search by a question or a phrase in a natural language, to find documents by meaning and to get the best passages of their text, use the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}).

## RetrieveAttributes property

The [RetrieveAttributes](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/retrieveattributes/) property allows you to retrieve the attributes associated with the found documents. If the property is set to true, the attributes of each found document are available in the [FoundDocument.Attributes](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/attributes/) property; otherwise, this property returns an empty array. The default value is false. For details on document attributes, see the [Document attributes]({{< ref "search/net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}) page.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string query = "Einstein";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Creating a search options instance
SearchOptions options = new SearchOptions();
options.RetrieveAttributes = true; // Enabling the retrieval of document attributes

// Search in the index
SearchResult result = index.Search(query, options);

// Printing the attributes of the found documents
for (int i = 0; i < result.DocumentCount; i++)
{
    FoundDocument document = result.GetFoundDocument(i);
    Console.WriteLine(document.DocumentInfo.FilePath + ": " + string.Join(", ", document.Attributes));
}
```

## SearchDocumentFilter property

The [SearchDocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/searchdocumentfilter/) property allows you to specify the filter of documents returned as a search result. By default, all documents found will be returned as a search result. For details, see the [Document filtering in search result]({{< ref "search/net/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}) page.

## SemanticExpansion property

The [SemanticExpansion](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/semanticexpansion/) property returns the options of the semantic expansion of queries, which allows you to find the words of the index that are close in meaning to the words of a query: their forms, synonyms and translations. The property is available since version 26.12. The words are found by a word vectors model, which is added to the index by the [AddModel](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/addmodel/) method. By default, the semantic expansion is disabled. See the [Semantic expansion of queries]({{< ref "search/net/developer-guide/advanced-usage/ai-search/semantic-query-expansion.md" >}}) page.

## SpellingCorrector property

The [SpellingCorrector](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/spellingcorrector/) property returns the spelling corrector options object, which allows you to enable spelling correction in search queries and specify spelling correction options. By default, spelling correction is disabled. See the [Spell checking]({{< ref "search/net/developer-guide/advanced-usage/searching/spell-checking.md" >}}) page for details.

## UseCaseSensitiveSearch property

The [UseCaseSensitiveSearch](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/usecasesensitivesearch/) property allows you to perform case-sensitive search. Please note that case-sensitive search cannot be combined with fuzzy search, synonym search, homophone search, word forms search, spelling correction, and keyboard layout correction (see [Incompatible options](#incompatible-options) below). By default, case-insensitive search is performed. For details, see the [Case sensitive search]({{< ref "search/net/developer-guide/advanced-usage/searching/case-sensitive-search.md" >}}) page.

## UseHomophoneSearch property

The [UseHomophoneSearch](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/usehomophonesearch/) property allows you to enable the homophone search feature. By default, the homophone search feature is disabled. For details, see the [Homophone search]({{< ref "search/net/developer-guide/advanced-usage/searching/homophone-search.md" >}}) page.

## UseSynonymSearch property

The [UseSynonymSearch](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/usesynonymsearch/) property allows you to enable the synonym search feature. By default, the synonym search feature is disabled. For details, see the [Synonym search]({{< ref "search/net/developer-guide/advanced-usage/searching/synonym-search.md" >}}) page.

## UseWordFormsSearch property

The [UseWordFormsSearch](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/usewordformssearch/) property allows you to enable the word forms search feature. By default, the word forms search feature is disabled. For details, see the [Search for different word forms]({{< ref "search/net/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}) page.

## Incompatible options

The search options are checked when a search starts. In the following cases, the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method does not throw an exception: it returns an empty search result and raises the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index with a message describing the problem:

*   Case-sensitive search is enabled together with fuzzy search, synonym search, homophone search, word forms search, spelling correction, keyboard layout correction, or semantic expansion.
*   Synonym search is enabled, but the synonym dictionary of the index is empty.
*   Homophone search is enabled, but the homophone dictionary of the index is empty.

A search query in text form with a syntax error is handled in the same way: the search returns an empty result, and the error message is passed to the ErrorOccurred event. Therefore, it is recommended to subscribe to the ErrorOccurred event to receive such messages. Since version 26.11, the [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) property of such a result also tells the problem by a line that starts with "Search failed: " (see the [Search results]({{< ref "search/net/developer-guide/advanced-usage/searching/search-results.md" >}}) page). For details on index events, see the [Search index events]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}) page.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
string query = "Einstein";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Subscribing to the event of errors
index.Events.ErrorOccurred += (sender, args) =>
{
    Console.WriteLine(args.Message);
};

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Creating a search options instance with incompatible options
SearchOptions options = new SearchOptions();
options.UseCaseSensitiveSearch = true;
options.FuzzySearch.Enabled = true;

// The search returns an empty result, and the ErrorOccurred event reports the reason
SearchResult result = index.Search(query, options);
Console.WriteLine("Document count: " + result.DocumentCount);
Console.WriteLine("Warnings: " + result.Warnings); // The reason after "Search failed: "
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
