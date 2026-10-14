---
id: search-index-settings
url: search/net/search-index-settings
title: Search index settings
weight: 3
description: "This page contains a description of all index settings that can be specified in an instance of the IndexSettings class."
keywords: index settings, description of all index settings
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains a description of all index settings that can be specified in an instance of the [IndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/) class.

An index settings object is passed to an [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) constructor when a new index is created. The settings are saved in the index. When an existing index is opened, it uses the settings saved in it; only the [CustomExtractors](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/customextractors/) and [Logger](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/logger/) settings, which are not saved, are taken from the passed settings object. The settings of an opened index are available through the [Index.IndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search/index/indexsettings/) property.

Some settings define how the text of documents is divided into terms, so they cannot be changed freely after the index is created:

*   The [IndexType](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/indextype/), [TextStorageSettings](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/textstoragesettings/), [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) settings are fixed when the index is created; a value assigned after that is ignored.
*   The [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) and [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/) settings can be changed only while the index contains no documents and is not performing an operation; otherwise, a change throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception).
*   The other settings can be changed at any time.

For details, see the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

## CustomExtractors property

The [CustomExtractors](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/customextractors/) property returns a collection of custom text extractors that allows adding new extractors for supported or not supported formats. Custom extractors are not saved in an index, so they must be added each time the index is created or opened. A complete example of implementing a custom text extractor and using it to extract text is presented on the page [Custom text extractors]({{< ref "search/net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}).

## DocumentFilter property

The [DocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/documentfilter/) property allows you to set a filter used to determine whether files being added to the index should be indexed. If a document added separately or located in an added folder does not match the filter, it is not indexed. The default value is null, which means that all added files will be indexed if their format is supported. The value can be changed at any time. Detailed information on creating and setting the document indexing filter can be found on the page [Document filtering during indexing]({{< ref "search/net/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}}).

## IndexType property

The [IndexType](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/indextype/) property is used to specify the type of an index. The [IndexType](https://reference.groupdocs.com/search/net/groupdocs.search.options/indextype/) enumeration contains 3 values:

*   **NormalIndex** - the type of an index containing the content and metadata of the added documents. It supports all search features.
*   **MetadataIndex** - the type of an index containing only metadata of added documents. It supports all search features. An example of creating an index of this type is presented on the [Indexing metadata of documents]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-metadata-of-documents.md" >}}) page.
*   **CompactIndex** - the type of an index containing the content and metadata of the added documents. It takes much less disk space, but does not support the [phrase search]({{< ref "search/net/developer-guide/advanced-usage/searching/phrase-search.md" >}}) and the [date range search]({{< ref "search/net/developer-guide/advanced-usage/searching/date-range-search.md" >}}) features.

The default value of this property is **NormalIndex**. The index type is fixed when the index is created.

## InMemoryIndex property

The read-only property [InMemoryIndex](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/inmemoryindex/) returns a value indicating whether an index is located in RAM or on disk. The value of this property is set indirectly when creating or loading an index. An index is created on disk when the path to the index folder is specified; otherwise, an index is created in memory. See the [Search index location]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-location.md" >}}) page for details.

## Logger property

The [Logger](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/logger/) property allows you to specify in the index settings the logger used for logging index events and errors during its operation. The default value is null meaning that logging is not used. The logger is not saved in an index, so it must be assigned each time the index is created or opened. Detailed information on creating and assigning an index logger and the implementation of a custom logger is presented on the page [Logging]({{< ref "search/net/developer-guide/advanced-usage/indexing/logging.md" >}}).

## MaxIndexingReportCount property

The [MaxIndexingReportCount](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/maxindexingreportcount/) property allows you to specify the maximum number of indexing reports stored in RAM for an index since it was created or loaded. The default value is 5. Detailed information on indexing reports is provided on the page [Indexing reports]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-reports.md" >}}).

## MaxSearchReportCount property

The [MaxSearchReportCount](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/maxsearchreportcount/) property allows you to specify the maximum number of search reports stored in RAM for an index since it was created or loaded. The default value is 10. Detailed information on search reports is provided on the page [Search reports]({{< ref "search/net/developer-guide/advanced-usage/searching/search-reports.md" >}}).

## ReplaceUnpairedSurrogates property

The [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) property specifies how unpaired UTF-16 surrogate characters in the text of documents are processed during indexing. If the value is true, each unpaired surrogate character is replaced with the Unicode replacement character (U+FFFD), so the characters around it are not joined into a single term. If the value is false, unpaired surrogate characters are removed, as previous versions did.

The default value is true. The setting is fixed when the index is created. An index updated from a version earlier than 26.10 has this setting disabled.

## SearchThreads property

The [SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/) property allows you to set the number of threads used to search in an index. By default, this value is [NumberOfThreads](https://reference.groupdocs.com/search/net/groupdocs.search.options/numberofthreads/).Default, which means that the search is performed using as many threads as there are processor cores. This number of threads ensures the optimal speed of each individual search in an index. If you plan a large number of parallel search queries and you need to ensure maximum total search performance, you should set the [NumberOfThreads](https://reference.groupdocs.com/search/net/groupdocs.search.options/numberofthreads/).One value for this property.

The value can be changed at any time, also for an existing index through the [Index.IndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search/index/indexsettings/) property. A search uses the value that is set when the search starts, and the new value is saved in the index. The setter throws [ArgumentOutOfRangeException](https://learn.microsoft.com/dotnet/api/system.argumentoutofrangeexception) if the value is not a member of the [NumberOfThreads](https://reference.groupdocs.com/search/net/groupdocs.search.options/numberofthreads/) enumeration.

In a multithreaded search, the order of the words found by a regular expression, numeric range, fuzzy or spelling correction search may differ from run to run. If you need a deterministic order of the results, use the [NumberOfThreads](https://reference.groupdocs.com/search/net/groupdocs.search.options/numberofthreads/).One value.

The following example demonstrates how to change the number of search threads of an existing index.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string query = "Einstein";

// Opening an existing index
Index index = new Index(indexFolder);

// Changing the number of threads used for the search; the new value is saved in the index
index.IndexSettings.SearchThreads = NumberOfThreads.One;

// Searching using the new number of threads
SearchResult result = index.Search(query);
```

## SkipBlendedOnlyTerms property

The [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) property specifies whether terms that consist only of [blended characters]({{< ref "search/net/developer-guide/advanced-usage/indexing/character-types.md#blended-characters" >}}) are skipped during indexing. For example, if the hyphen is a blended character, a standalone hyphen in the text is not indexed as a term when the value is true. This reduces the number of meaningless terms in the index.

The default value is true. The setting is fixed when the index is created. An index updated from a version earlier than 26.10 has this setting disabled.

The following example demonstrates how to create an index that keeps the terms consisting only of blended characters.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating index settings
IndexSettings settings = new IndexSettings();
settings.SkipBlendedOnlyTerms = false; // Keeping the terms consisting only of blended characters

// Creating an index with the specified settings
Index index = new Index(indexFolder, settings, true);

// Indexing documents from the specified folder
index.Add(documentsFolder);
```

## TextStorageSettings property

The [TextStorageSettings](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/textstoragesettings/) property allows you to specify the settings for saving the text of indexed documents in an index. The default value is null, which means that document texts are not stored. The setting is fixed when the index is created. Detailed information on saving the text of indexed documents in an index is presented on the page [Storing text of indexed documents]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}).

## UseCharacterReplacements property

The [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/) property allows you to set the value indicating whether to perform character replacements during the indexing process or not. The default value is false. The value can be changed only while the index contains no documents and is not performing an operation; otherwise, [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) is thrown. Details on replacing characters during indexing are provided on the page [Character replacement during indexing]({{< ref "search/net/developer-guide/advanced-usage/indexing/character-replacement-during-indexing.md" >}}).

## UseStopWords property

The [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) property allows you to specify a value indicating whether to use stop words during indexing or not. Stop words are frequently used words that do not carry a semantic load, which are removed from an index to reduce its size. The default value for this property is true. The value can be changed only while the index contains no documents and is not performing an operation; otherwise, [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) is thrown. Detailed information on stop words and their use is presented on the page [Indexing with stop words]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-with-stop-words.md" >}}).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
