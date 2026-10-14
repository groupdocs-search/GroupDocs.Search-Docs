---
id: migration-notes
url: search/net/migration-notes
title: Migration Notes
weight: 3
description: "This article describes how to migrate applications to GroupDocs.Search for .NET 26.10 with its new index format, and how to migrate code to the API introduced in version 19.9."
keywords: migration, upgrade, 26.10, index format, IndexUpdater, update index version, breaking changes, 19.9 API
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page describes the changes to take into account when upgrading an application to a new version of GroupDocs.Search for .NET:

*   [Migrating to version 26.10](#migrating-to-version-2610) — the new index format and the behavior changes that may require changes in your code.
*   [Migrating to the API of version 19.9](#migrating-to-the-api-of-version-199) — the changes of the public API introduced in version 19.9.

## Migrating to version 26.10

### What is new in the index format

Version 26.10 introduces a new format for storing an index on disk. Compared with previous versions:

*   the index size on disk grows in proportion to the indexed data, and a loaded index requires about half as much RAM;
*   an index is opened 2 to 4 times faster, and search in an index with many segments reads only the segments that contain the query terms;
*   index data are protected with checksums, and all changes to an index are applied atomically: if an operation is interrupted by an error, a power failure or termination of the process, the index remains in its previous consistent state;
*   deleting documents no longer rewrites the index, and searches and index-modifying operations no longer block each other.

The full rules of compatibility between the new format and indexes of previous versions are described on the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

### Opening indexes of previous versions

Version 26.10 opens only indexes of the new format. The [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) constructors and the [LoadIntoMemoryCompletely](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) method throw [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) for an index created by a previous version, including versions 26.7 and 26.8. Such an index must be updated with the [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) class first:

*   An index created by versions 22.7–26.8 is converted to the new format without re-indexing; the source documents are not required.
*   An index created by versions 18.5–22.6 is re-indexed: the documents must be available at the paths from which they were indexed.
*   An index created by a version earlier than 18.5 cannot be updated; create a new index and add the documents to it.

The updated index is written to another folder; the source index is not modified. Conversely, previous versions cannot open an index of version 26.10.

The following example demonstrates how to update an index of a previous version before opening it.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\"; // The folder of an index created by a previous version
string updatedIndexFolder = @"c:\MyUpdatedIndex\"; // A new empty folder for the updated index

IndexUpdater updater = new IndexUpdater();
if (updater.CanUpdateVersion(indexFolder))
{
    // The source index is not changed; the content of the target folder is deleted before the update
    VersionUpdateResult result = updater.UpdateVersion(indexFolder, updatedIndexFolder);
    if (result == VersionUpdateResult.Updated)
    {
        using (Index index = new Index(updatedIndexFolder))
        {
            SearchResult searchResult = index.Search("Einstein");
            Console.WriteLine("Documents found: " + searchResult.DocumentCount);
        }
    }
}
```

See [Update index version]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md#update-index-version" >}}) and [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) for the rules of the target folder, the values of [VersionUpdateResult](https://reference.groupdocs.com/search/net/groupdocs.search.common/versionupdateresult/) and the exceptions.

### Upgrade checklist

*   **Upgrade all applications together.** All applications that use the same index folder, including all nodes of a [search network]({{< ref "search/net/developer-guide/advanced-usage/scaling/_index.md" >}}), must be upgraded to version 26.10 at the same time. Update the index of each shard or create the shards anew before starting the nodes.
*   **Update indexes into new folders.** Update each index with [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) into a new folder that is not the source folder, does not contain it and is not inside it; then switch the application to the new folder. See [Update index version]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md#update-index-version" >}}).
*   **Configure term dictionaries before adding documents.** The alphabet, the tokenization dictionary, the character replacements, the stop words and the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) and [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/) settings can be changed only while the index contains no documents. See [Settings of an updated index]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#settings-of-an-updated-index" >}}).
*   **Merge updated indexes into compatible indexes.** An index updated from a previous version has the [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) settings disabled. To merge it into a new index, create the new index with both settings set to *false*. See [Merging compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#merging-compatibility" >}}).
*   **Check the number of search threads.** Search now uses as many threads as there are processor cores by default. To restore single-threaded search with a deterministic order of results, set [SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/) to *NumberOfThreads.One*. See [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}).
*   **Optimize the index to free disk space after deletions.** Deleted documents are excluded from search immediately but are physically removed only by [Optimize](https://reference.groupdocs.com/search/net/groupdocs.search/index/optimize/). See [Optimize index]({{< ref "search/net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}).
*   **Iterate over chunks by the token.** The number of chunks of a search by chunks is not known in advance; continue while [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/nextchunksearchtoken/) is not *null*. See [Search by chunks]({{< ref "search/net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}).
*   **Empty the backup folder of SaveTo once.** [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) does not overwrite an index of a previous version. If an application regularly saves its index to the same folder, empty that folder once after the upgrade. See [Saving an index to another folder]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#saving-an-index-to-another-folder" >}}).
*   **Check documents interrupted by cancellation.** A document whose indexing was interrupted by cancellation is marked with [IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/) and is indexed again by the next [Update](https://reference.groupdocs.com/search/net/groupdocs.search/index/update/). See [Indexing options]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-options.md" >}}).

### Breaking changes

#### Term dictionaries are fixed once an index contains documents

The [Alphabet](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/alphabet/), [TokenizationDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/tokenizationdictionary/), [CharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/characterreplacements/) and [StopWordDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/stopworddictionary/) dictionaries, as well as the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) and [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/) settings, can be changed only while the index contains no documents and is not performing an operation. Otherwise, a change throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception); a call that does not change the content is allowed. Previously such a change was accepted, and documents indexed before and after it were processed by different rules. The other dictionaries (synonyms, homophones, aliases, the spelling corrector and document passwords) can be changed at any time.

Code that changes these dictionaries of an existing index must be moved to the place where the index is created. The following example demonstrates how to configure the dictionaries of a new index before adding documents.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating a new index; an existing index in the folder is overwritten
Index index = new Index(indexFolder, true);

// Configuring the term dictionaries while the index contains no documents
index.Dictionaries.Alphabet.SetRange(new char[] { '-' }, CharacterType.Blended);
index.Dictionaries.StopWordDictionary.AddRange(new string[] { "and", "or", "the" });

// Indexing documents; after this, the dictionaries above cannot be changed
index.Add(documentsFolder);
```

#### Incompatible indexes are not merged

The [Merge](https://reference.groupdocs.com/search/net/groupdocs.search/index/merge/) method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) before making any changes if the merged indexes divide text into terms differently: they have different alphabets, tokenization dictionaries, stop words or character replacements, or different values of the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/), [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/), [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) or [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) settings. The exception is thrown also when [MergeOptions.IsAsync](https://reference.groupdocs.com/search/net/groupdocs.search.options/mergeoptions/isasync/) is *true*. In addition, the term dictionaries of the current index are no longer changed by merging, and a merge is not performed if a merged index is performing an operation or has failed; the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event reports it.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string otherIndexFolder = @"c:\MyOtherIndex\";

Index index = new Index(indexFolder);
Index otherIndex = new Index(otherIndexFolder);
try
{
    index.Merge(otherIndex, new MergeOptions());
}
catch (InvalidOperationException exception)
{
    // The indexes divide text into terms differently; the message names the differences
    Console.WriteLine(exception.Message);
}
```

See [Merge indexes]({{< ref "search/net/developer-guide/advanced-usage/indexing/merge-indexes.md" >}}) and [Merging compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#merging-compatibility" >}}).

#### Search is multithreaded by default

With the default value of [IndexSettings.SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/), search now uses as many threads as there are processor cores; previously only one thread was used. As a result, the order of the terms found by regular expression, numeric range, fuzzy search and spelling correction may differ from run to run, and so may the order of search results that depends on it. The property can be changed at any time through the [Index.IndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search/index/indexsettings/) property, and the new value is saved in the index; a value that is not a member of [NumberOfThreads](https://reference.groupdocs.com/search/net/groupdocs.search.options/numberofthreads/) throws [ArgumentOutOfRangeException](https://learn.microsoft.com/dotnet/api/system.argumentoutofrangeexception).

The following example demonstrates how to restore single-threaded search with a deterministic order of results.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

Index index = new Index(indexFolder);
index.IndexSettings.SearchThreads = NumberOfThreads.One; // The value is saved in the index

SearchResult result = index.Search("^relativ.*");
```

#### New index settings change which terms are indexed

The new [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) settings are enabled by default in a new index and are fixed when the index is created: unpaired surrogate characters are replaced with the Unicode replacement character (U+FFFD) instead of being removed, and terms that consist only of blended characters are not indexed. To keep the behavior of previous versions, set both settings to *false* before creating an index, as shown on the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#merging-compatibility" >}}) page. An index updated from a previous version has both settings disabled.

Other changes in text processing:

*   Terms longer than 80 characters are not indexed.
*   Letter case is converted in the same way on .NET Framework and .NET, so search results no longer depend on the platform.
*   The lookup methods of the stop word, synonym and homophone dictionaries ignore letter case; words are kept in lower case.

#### Search by chunks returns a different number of chunks

A chunk no longer corresponds to an index segment: only the segments that may contain matches are searched, and the number of chunks is not known in advance. Code that relies on the number of segments must iterate over the chunks while [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/nextchunksearchtoken/) is not *null*.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

Index index = new Index(indexFolder);
SearchOptions options = new SearchOptions();
options.IsChunkSearch = true;

SearchResult result = index.Search("Einstein", options);
int documentCount = result.DocumentCount;
while (result.NextChunkSearchToken != null)
{
    result = index.SearchNext(result.NextChunkSearchToken);
    documentCount += result.DocumentCount;
}
Console.WriteLine("Documents found: " + documentCount);
```

See [Search by chunks]({{< ref "search/net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}).

#### Deleted documents are removed from disk by optimization

Deleting documents ([Delete](https://reference.groupdocs.com/search/net/groupdocs.search/index/delete/), or [Update](https://reference.groupdocs.com/search/net/groupdocs.search/index/update/) for deleted files) no longer rewrites the index. Deleted documents are excluded from search immediately, but their data remain on disk until the index is optimized with the [Optimize](https://reference.groupdocs.com/search/net/groupdocs.search/index/optimize/) method. Optimizing a very large index may leave several segments. See [Delete indexed documents]({{< ref "search/net/developer-guide/advanced-usage/indexing/delete-indexed-documents.md" >}}) and [Optimize index]({{< ref "search/net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}).

#### Searches are not blocked by index modification

Searches and index-modifying operations (indexing, update, deletion, merging, optimization) no longer block each other. Each search works with a snapshot of the index taken when the search starts, so a search started during indexing does not find the documents being added. See [Search thread safety]({{< ref "search/net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}}).

#### SaveTo does not overwrite an index of another version

The [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) and does not change the target folder if the folder contains an index of another version or a damaged index, or if it is the folder of the index itself. An index of the current version in the target folder is replaced. An application that saves its index to the same folder, for example as a backup, has to empty that folder once after the upgrade.

#### Documents interrupted by cancellation are marked as indexed with error

If an indexing operation is cancelled, a document whose indexing was interrupted gets [DocumentInfo.IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/) = *true* and is indexed again by the next [Update](https://reference.groupdocs.com/search/net/groupdocs.search/index/update/). Documents added from streams, structures or extracted data cannot be re-read by the update and have to be added again.

## Migrating to the API of version 19.9

### Why migrate?

Here are the main reasons for using the updated API provided by GroupDocs.Search for .NET since version 19.9:

*   Unified work with some similar entities to increase the intuitiveness of using API.
*   The architecture of the product has been revised and optimized, so that some functions will work faster.
*   Some entities have been renamed to improve code readability.
*   The changes made to the API are not too significant, so the migration will not be too difficult.
*   New functionality is added only to the new API.

### How to migrate?

The following code examples demonstrate changes in the use of the API.

API before version 19.9 (does not compile with the current version):

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.AddToIndex(documentsFolder);
 
// Creating a search parameters object
SearchParameters parameters = new SearchParameters();
parameters.FuzzySearch.Enabled = true; // Enabling the fuzzy search
parameters.FuzzySearch.FuzzyAlgorithm = new TableDiscreteFunction(2); // Setting the number of possible differences for each word
 
// Searching in the index
SearchResults results = index.Search("Happiness", parameters);
 
if (results.Count > 0)
{
    // Generating HTML-formatted text of a document with highlighted search results
    index.HighlightInText(@"c:\Highlighted.html", results[0]);
}
```

API since version 19.9:

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Creating a search options object
SearchOptions options = new SearchOptions();
options.FuzzySearch.Enabled = true; // Enabling the fuzzy search
options.FuzzySearch.FuzzyAlgorithm = new TableDiscreteFunction(2); // Setting the number of possible differences for each word
 
// Searching in the index
SearchResult result = index.Search("Happiness", options);
 
if (result.DocumentCount > 0)
{
    // Generating HTML-formatted text of a document with highlighted search results
    OutputAdapter outputAdapter = new FileOutputAdapter(OutputFormat.Html, @"c:\Highlighted.html");
    Highlighter highlighter = new DocumentHighlighter(outputAdapter);
    index.Highlight(result.GetFoundDocument(0), highlighter);
}
```
