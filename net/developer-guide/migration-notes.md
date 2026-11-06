---
id: migration-notes
url: search/net/migration-notes
title: Migration Notes
weight: 3
description: "This article describes how to migrate applications to GroupDocs.Search for .NET 26.11 and to version 26.10 with its new index format, and how to migrate code to the API introduced in version 19.9."
keywords: migration, upgrade, 26.11, 26.10, index format, IndexUpdater, update index version, breaking changes, 19.9 API
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page describes the changes to take into account when upgrading an application to a new version of GroupDocs.Search for .NET:

*   [Migrating to version 26.11](#migrating-to-version-2611) — the behavior changes that may require changes in your code.
*   [Migrating to version 26.10](#migrating-to-version-2610) — the new index format and the behavior changes that may require changes in your code.
*   [Migrating to the API of version 19.9](#migrating-to-the-api-of-version-199) — the changes of the public API introduced in version 19.9.

## Migrating to version 26.11

Version 26.11 opens the indexes of version 26.10 without an update. This section lists the behavior changes of version 26.11 that may require changes in your code. For the full list of changes, see the [release notes](https://releases.groupdocs.com/search/net/release-notes/).

### Upgrade checklist

*   **Upgrade all applications together.** An index created by version 26.11, or an index of version 26.10 that version 26.11 has changed (for example, by indexing, deletion of documents, optimization or a change of its settings), cannot be opened by earlier versions, and the nodes of a search network of version 26.11 do not connect to the nodes of earlier versions. Upgrade all applications that use the same index folder and all nodes of a search network at the same time. See [Indexes of version 26.11]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#indexes-of-version-2611" >}}).
*   **Dispose of an index before its folder is used again.** An index folder is used by one [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) object at a time; see [Index folder and lifetime](#index-folder-and-lifetime).
*   **Create new indexes in empty folders.** A new index is not created in a folder that contains other files or subfolders unless *overwriteIfExists* is *true*.
*   **Get the errors of searches from the event or the warnings.** A search no longer throws the exception of its failure; see [Search](#search).
*   **Iterate over chunks by the token.** A search by chunks no longer returns a chunk for every index segment; see [Search by chunks](#search-by-chunks).
*   **Close the streams of StreamOutputAdapter yourself.** See [Highlighting and document text](#highlighting-and-document-text).
*   **Rebuild the program of the extraction in a separate process** with version 26.11; see [Extraction in a separate process](#extraction-in-a-separate-process).
*   **Index some documents again** to get the new behavior: the HTML documents, the documents indexed with a custom word splitter, the containers with nested items and the documents with metadata values of other types, such as decimal numbers; see [Indexing](#indexing).
*   **In the evaluation mode, enumerate at most 10 found documents.** See [Evaluation mode](#evaluation-mode).

### Behavior changes

#### Index folder and lifetime

*   **One Index object per folder.** The [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) constructors, the [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) and [IndexUpdater.UpdateVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/updateversion/) methods, and the [Create](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/create/) and [AddToRepository](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/addtorepository/) methods of an index repository throw [IOException](https://learn.microsoft.com/dotnet/api/system.io.ioexception) for a folder that another Index object of version 26.11 or later uses, in this process or in another one. Dispose of an Index object, for example with a *using* statement, before its folder is used again: an Index object that is not disposed keeps its folder until the garbage collector finalizes it, which may not happen before the process ends. See [Search index location]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-location.md#index-folder" >}}).
*   **Index.Dispose cancels the running operations and waits until they end.** An asynchronous operation no longer finishes after [Dispose](https://reference.groupdocs.com/search/net/groupdocs.search/index/dispose/). A handler of an event can dispose of the index; Dispose does not wait then. See [Index lifecycle]({{< ref "search/net/developer-guide/basic-usage/index-lifecycle.md" >}}).
*   **A disposed index throws ObjectDisposedException.** The methods that index, search, change or save a disposed index throw [ObjectDisposedException](https://learn.microsoft.com/dotnet/api/system.objectdisposedexception), and a method that changes a dictionary of a disposed index throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception); the properties of the index and the information about its documents remain available.
*   **A copy of the folder of an open index.** On Windows, one file of the folder of an open index cannot be read by other programs, so a program that copies the folder, for example a backup program, reports that file; it need not be copied. [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) copies an open index.
*   **A new index is not created in a folder with other content.** The constructors of Index with a folder, unless *overwriteIfExists* is *true*, throw [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) for a folder that contains no index but files or subfolders that are not a part of an index; previously, they deleted them. Create the index in an empty folder, or set *overwriteIfExists* to *true* to delete the content of the folder.

#### Opening and saving an index

*   **An index of a newer version is refused with InvalidOperationException.** The constructors of Index and [LoadIntoMemoryCompletely](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) throw InvalidOperationException instead of [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception) for an index that a newer version of the library has created or changed, and the index is not changed; an index with a dictionary file of a newer version is refused as well, and the file is kept. [CheckSegments](https://reference.groupdocs.com/search/net/groupdocs.search/index/checksegments/) reports through the ErrorOccurred event that such an index is not checked, and repairs nothing.
*   **An index that has lost the files of its changes throws InvalidDataException.** Previously, it was refused with InvalidOperationException as an index of a version that cannot be updated. Code that catches InvalidOperationException to learn that an index cannot be opened has to catch InvalidDataException as well; such an index is restored from a backup or created again. See [Opening an index]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#opening-an-index" >}}).
*   **An index whose last operation could not be merged is opened without the changes of that operation.** The constructor no longer throws an exception for such an index; the error is written to the log of the index and to the next indexing report. Repeat the operation.
*   **SaveTo of a busy index throws InvalidOperationException.** Previously, it raised the ErrorOccurred event and saved nothing. Save the index when its operation has finished, for example after the [OperationFinished](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/operationfinished/) event.
*   **SaveTo of an index whose file is missing throws InvalidDataException** before it changes the target folder; previously, it wrote an index that did not work. Repair the index with [CheckSegments](https://reference.groupdocs.com/search/net/groupdocs.search/index/checksegments/) or index its documents again, and save it again.
*   **SaveTo of an index with the Failed status saves the index** in the state that it has worked with. Load the saved index to continue working with it.
*   **A change of the index whose saving fails is undone.** This concerns the changes of the attributes, the dictionaries and the settings of an index, the notifications about renamed files, the optimization and the repair: repeat the change. An index that cannot undo such a change reports a critical error and gets the Failed status: dispose of it and create a new Index object for its folder. See [Search index events]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}).
*   **An optimization or a merge of indexes that reads damaged data fails** and keeps the index as it was; previously, it made the damage a part of the new files. Repair the index with [CheckSegments](https://reference.groupdocs.com/search/net/groupdocs.search/index/checksegments/) with the repair enabled, and repeat the operation. An indexing or a merge of indexes that finds damaged data of the index that can be made again from its segments makes the data again and reports an error that is not critical.

#### Indexing

*   **Document filters.** A string of the document filter of the index settings can have up to 65535 characters: the constructor that creates an index with a longer string throws ArgumentException. CreateNot, CreateAnd and CreateOr of [DocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search.options/documentfilter/) and [SearchDocumentFilter](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchdocumentfilter/) throw ArgumentException when the filter that they create would be nested more than 128 levels deep.
*   **Time and extension filters.** A time bound of the kind DateTimeKind.Utc is compared with the time of a file in the universal time; an extension passes a file whose name ends with the extension after a dot (for example, "doc" no longer passes "mydoc"), and an empty extension passes only the files without an extension. The next update of an index adds and deletes the files according to the changed filter. A path that the regular expression of a filter does not finish matching in time is not indexed and is reported through ErrorOccurred. See [Document filtering during indexing]({{< ref "search/net/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}}).
*   **The links to folders inside the added folders are not followed.** The files that an index has indexed under such a link are deleted from the index by the next [Update](https://reference.groupdocs.com/search/net/groupdocs.search/index/update/); add the target of the link to index them.
*   **An item of a container that cannot be read no longer stops the indexing of the items after it.** Every such item raises its own ErrorOccurred event, whose message names the file and the item in it. The container items nested more than 32 levels deep are not indexed.
*   **A document that is too large for a segment of the index fails with an error.** Split such a document, for example a log of several gigabytes, into smaller documents.
*   **A document whose text fails while it is read is marked as indexed with an error** ([IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/)) also with a custom word splitter, in [Extractor](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/) and in the extraction in a separate process; the next Update indexes such a file again.
*   **Documents of files.** A document created by [Document.CreateFromFile](https://reference.groupdocs.com/search/net/groupdocs.search.common/document/createfromfile/) is indexed with its attributes and additional fields, and the loader of a lazy document of a file is called. [Update](https://reference.groupdocs.com/search/net/groupdocs.search/index/update/) indexes a changed file again with the attributes of its indexed version, which a handler of the [FileIndexing](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/fileindexing/) event sees and can replace; the additional fields of its document are not kept.
*   **IDocumentLoader.CloseDocument is called also for a loaded document that does not match the lazy document.** Previously, [CloseDocument](https://reference.groupdocs.com/search/net/groupdocs.search.common/idocumentloader/closedocument/) was called only for a document that matched.
*   **An index in memory keeps the large texts in memory while it indexes them.** The text of a field of more than about 5 million characters takes about 2 bytes per character until the text of the document is written; previously, it was buffered in a temporary file.
*   **UseBackupPrivilege.** A file that [UseBackupPrivilege](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/usebackupprivilege/) cannot read is indexed with an error, and an index in memory and an Extractor without [ExtractorSettings.TempFolder](https://reference.groupdocs.com/search/net/groupdocs.search.common/extractorsettings/tempfolder/) copy such files into the temporary folder of the system. The privilege is disabled again after the operations that use it, and an account without the privilege is reported by ErrorOccurred.
*   **Code pages of Windows on .NET.** The library registers the provider of the code pages of .NET (CodePagesEncodingProvider) for the process when it first reads a text document. After that, Encoding.GetEncoding of the application gets the code pages of Windows as well, and on Windows Encoding.GetEncoding(0) gives the code page of the system instead of UTF-8.
*   **HTML documents.** The addresses of the links and the labels of the images and of the media are not indexed, and an HTML document without a byte order mark is read in the encoding that [IndexingOptions.Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/encoding/) or the FileIndexing event sets, else in the charset of its meta tag, else in UTF-8. Index again the HTML documents that an earlier version has indexed. See [Supported document formats]({{< ref "search/net/getting-started/supported-document-formats.md" >}}).
*   **Other documents to index again.** The documents that an earlier version has indexed with a custom word splitter keep their words without the character replacements of the index and, in an index that stores the texts, the text of their words only; the items of containers with nested items keep their numbers of terms, which give a wrong relevance; the metadata values of other types, such as decimal numbers, keep the format of the culture of the indexing. Index such documents again.
*   **Custom container extractors.** [IContainerItemExtractor.GetContainerItems(Stream)](https://reference.groupdocs.com/search/net/groupdocs.search.common/icontaineritemextractor/getcontaineritems/) gets a stream that can seek at the position where GetFields(Stream) got it; previously, at the position where GetFields had left it.
*   **ExtractedData.Deserialize refuses damaged data.** [Deserialize](https://reference.groupdocs.com/search/net/groupdocs.search.common/extracteddata/deserialize/) throws InvalidDataException for a wrong number of items, for container items nested more than 128 levels deep and for a date out of its range, and InvalidOperationException for data that ends too early.

#### Search

*   **A search does not throw the exception of its failure.** [Index.Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) with a query in the object form and [SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search/index/searchnext/) report a failure by the ErrorOccurred event and return an empty result whose [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) start with "Search failed: "; previously, they threw the exception. A part of a query that cannot be used adds a warning that starts with "Query part not searched: ", and a query that is null throws ArgumentNullException.
*   **A result that lacks a part of the index is truncated.** A search that cannot read a part of the index returns a result whose [Truncated](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/truncated/) property is true and whose warnings tell the part and the cause.
*   **SearchThreads is the number of threads of all the searches of an index together.** One search uses as many threads as before, but K searches performed at the same time use max(K, SearchThreads) threads instead of K × SearchThreads. See [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md#searchthreads-property" >}}).
*   **The order of the words found with several search threads no longer differs from run to run.** The words found by a regular expression, numeric range, fuzzy or spelling correction search are in the order of a search in one thread whatever the value of SearchThreads, so [NumberOfThreads](https://reference.groupdocs.com/search/net/groupdocs.search.options/numberofthreads/).One is not needed for a deterministic order of the results.
*   **More results for the containers in a repository.** A search of an [index repository]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-repository.md" >}}) of several indexes returns each found item of a container; previously, it returned one item of each container.
*   **Limits of occurrences.** A search that reaches [MaxTotalOccurrenceCount](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/maxtotaloccurrencecount/) or [MaxOccurrenceCountPerTerm](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/maxoccurrencecountperterm/) finds no documents without occurrences, its warnings have one line about the limit, and Truncated is true also when the limit lets a part of the query search nothing or the query excluded with NOT is truncated. The limits count an occurrence of a part of a date of a date range once, as an occurrence of any term.
*   **A term that several parts of a query find is counted once** in the occurrences and in the relevance of a document, so the order of the results by relevance may change.
*   **Queries with stop words can find other documents.** A query that excludes a stop word with NOT, or that combines stop words only with AND or OR in a part of it, finds the documents of its other words.
*   **Character replacements.** In an index with [character replacements]({{< ref "search/net/developer-guide/advanced-usage/indexing/character-replacement-during-indexing.md" >}}), the words of a search get the replacements too, so a word, a synonym, a homophone, a correction or a word form with a replaced character finds more documents, and a word whose form after the replacements is that of a stop word is ignored. The words of a custom word splitter get the replacements as well.
*   **Word patterns.** A backslash in a word pattern begins an escape sequence, and a word pattern whose wildcards allow more than about 4000 characters in total finds nothing and is reported by ErrorOccurred; previously, Index.Search threw ArgumentException. In a case-sensitive search, a word pattern or a regular expression finds the words in any letter case, and the warnings of the result tell it.
*   **A search query nested more than 128 levels deep is not searched.** The AND and OR queries nested in one another are not counted. The search of a deeper query finds nothing and reports the error by ErrorOccurred.
*   **An alias in the text of another alias is not replaced.** The text of an alias is inserted into the query as it is.
*   **Date ranges.** A [date format](https://reference.groupdocs.com/search/net/groupdocs.search.options/dateformat/) created without a culture, as the default formats are, uses the Gregorian calendar and the English names of the months; to search for the dates of a culture, create the format by DateFormat(string, DateFormatElement[]). The dates with capitalized names of the months, the dates of the last day of a range whose start has a time of the day and the last month of a format without days are found now.
*   **The text of a numeric range query does not depend on the culture.** SearchQuery.ToString writes a negative number with the hyphen-minus in every culture.
*   **New exceptions for the values that an index or a search cannot use.** A compression that is not a member of Compression, a limit of the occurrences less than 1, a null provider of the word forms, a code point less than 0 or greater than 0x10FFFF and a type that is not a member of CharacterType throw ArgumentOutOfRangeException or ArgumentNullException at the assignment.
*   **SearchImage.Create throws ArgumentOutOfRangeException** for an index of an image that the file or the stream does not have, also for a negative index, and ArgumentException for a file or a stream without an image; previously, [SearchImage.Create](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchimage/create/) threw IndexOutOfRangeException.

#### Search by chunks

A search by chunks no longer returns a chunk for every index segment: the segments in which the query can find nothing are passed over, and the number of chunks can differ from search to search. Code that relies on the number of segments must iterate over the chunks while [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/nextchunksearchtoken/) is not *null*.

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

A search by chunks of an index repository goes index by index: [IndexRepository.Search](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/search/) returns the first chunk of the first index with the token of the next chunk, and [IndexRepository.SearchNext](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/searchnext/) continues the search; previously, a repository of several indexes returned the first chunks of all its indexes in one result without a token. To find all the documents in one result, search without chunks. A search of a repository with a query that is null throws ArgumentNullException, also for a repository without indexes, and the warnings of its result have no empty lines.

See [Search by chunks]({{< ref "search/net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) and [Search by chunks in an index repository]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-repository.md#search-by-chunks-in-an-index-repository" >}}).

#### Highlighting and document text

*   **Highlight and GetDocumentText read the documents of the index only.** A found document or a document info of a document that the index does not contain, for example a deleted document, gives no text, and the ErrorOccurred event reports "Document is not found."; previously, an index that does not store the texts extracted the file at its path. An application that gets found documents or document infos from its users must check that a user may read the document.
*   **A term is highlighted in the fields where it is found.** A query of a field highlights nothing in the other fields, and the fragments of a field hold the words of that field only.
*   **Close the stream of a StreamOutputAdapter yourself**, for example with a *using* statement. [StreamOutputAdapter](https://reference.groupdocs.com/search/net/groupdocs.search.common/streamoutputadapter/) leaves the stream open; previously, it closed the stream after the first output.
*   **The highlighting reads an HTML document in the encoding of its indexing.** [TextOptions.Encoding](https://reference.groupdocs.com/search/net/groupdocs.search.options/baseoptions/encoding/) is taken only for an HTML document whose encoding the index does not keep.
*   **Deserialize refuses damaged data with InvalidDataException.** [FoundDocument.Deserialize](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/deserialize/), DocumentInfo.Deserialize and FoundDocumentField.Deserialize throw InvalidDataException for data that the library does not write; such data must come from a trusted source.

See [Highlighting search results]({{< ref "search/net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}).

#### Dictionaries

*   **Only a new index gets the extended default alphabet.** The default [alphabet]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}) and tokenization dictionary of a new index have more scripts, while an index keeps the alphabet that it was created with, so [Merge](https://reference.groupdocs.com/search/net/groupdocs.search/index/merge/) refuses a new index and an index of a previous version. Give the new index the alphabet and the tokenization dictionary of the earlier index before documents are added to it, as shown on the [Alphabet]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}) page; see also [Merging compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#merging-compatibility" >}}). A search in an index repository or a search network finds the words of these scripts differently in the indexes created before and after the upgrade.
*   **The dictionaries refuse the words and texts that they cannot save.** The synonym, homophone, alias, document password and stop word dictionaries throw ArgumentException, and the call changes nothing, for a word, a text or a document key that they cannot save: for example, one with an unpaired surrogate, a word of a group or an alias text with a line break, a word of a group that is null, or an alias text that is empty. The empty and repeated words of a group of synonyms or homophones are left out.
*   **A merge and an update of indexes no longer add synonyms and homophones.** After Index.Merge and IndexUpdater.UpdateVersion, the synonyms and homophones of a word are those of the merged dictionaries.
*   **An import that cannot be completed changes nothing.** [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) of a file with an invalid alias, a password of a document that the dictionary already has, or an alphabet or character replacements with a value that is not a code point, adds nothing and reports the error by ErrorOccurred.
*   **The spelling corrector takes words of 1 to 80 characters without control characters.** [AddRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/spellingcorrector/addrange/) and the import skip the other words and report their number by ErrorOccurred; when an index is opened, the longer words of a dictionary saved by an earlier version are left out.

#### Events and reports

*   **An exception of an event handler is not passed to the caller of the operation.** It is written to the log of the index ([IndexSettings.Logger](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/logger/)), and the operation continues. To stop an operation from an event handler, use [Cancellation](https://reference.groupdocs.com/search/net/groupdocs.search.common/cancellation/).
*   **OperationFinished is raised at the end of every indexing operation.** A handler that counts the events gets one also for an update that has nothing to do and for an addition whose preparation fails.
*   **While the attributes of documents are changed, the status of the index is IndexStatus.ChangingAttributes**; previously, it was IndexStatus.Renaming.
*   **A merged index stays in its repository.** The handlers of the events of an [index repository]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-repository.md" >}}) get the events of an index also after the index is merged into another one.
*   **The last OptimizationProgressChanged event of an optimization comes when the optimization is done**, before the optimized index is saved, instead of before its last stage.
*   **The repair raises an event for every deleted document.** After the events that describe the repair, CheckSegments(true) raises ErrorOccurred once for every document that it deletes, with the key of the document and the reason; a document added from a stream or a structure has to be added again.
*   **An indexing report lists the errors reported since the previous indexing report.** [IndexingReport.Errors](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/errors/) no longer contains the errors of the earlier operations and of the searches, and it lists at most 10000 errors; every error is reported by ErrorOccurred.
*   **IndexedDocumentsSize is the total size of the indexed documents**, not the size of the last one, and [TotalIndexSize](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/totalindexsize/) does not count the files behind the links to folders inside the folder of the index.

See [Search index events]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}) and [Indexing reports]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-reports.md" >}}).

#### Extraction in a separate process

*   **Build the program of the process of the extraction with version 26.11.** The processes exchange new messages, which a program of an earlier version does not answer: each document then waits for [SeparateProcessOptions.Timeout](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/timeout/) and gets the default encoding.
*   **The text of the images is recognized by ExtractionHost.OcrConnector.** Set [OcrConnector](https://reference.groupdocs.com/search/net/groupdocs.search/extractionhost/ocrconnector/) in the program of the process of the extraction; the OCR connector of the indexing options is not used there. A process of the extraction without a connector is reported by ErrorOccurred, once per operation, when the indexing options enable the recognition.
*   **SeparateProcessOptions.Timeout also limits the start of the process of the extraction**, which has to connect to the index within this time. The documents are extracted in the separate process one at a time, whatever the value of [IndexingOptions.Threads](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/threads/).
*   **A document indexed with the extraction in a separate process is a file.** Its [DocumentSourceKind](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/documentsourcekind/) is File, not ExtractedData, and it is deleted by Delete with the paths of files.
*   **The PasswordRequired event is not raised for a document whose password is in the dictionary**, with the extraction in a separate process as without it.

See [Extraction in separate process]({{< ref "search/net/developer-guide/advanced-usage/indexing/extraction-in-separate-process.md" >}}).

#### Search network

*   **All the nodes of a search network are upgraded at once.** The nodes of version 26.11 and of earlier versions do not connect, and the log of a node names the reason.
*   **To protect a network, set the same NetworkKey on all its nodes.** Nodes with different keys, or a node with a key and a node without one, do not connect. The messages are not encrypted, a node without a key writes a warning to its log, and [TcpSettings.ConnectTimeout](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/connecttimeout/) also limits the start of a connection. See [TcpSettings.NetworkKey](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/networkkey/).
*   **The calls of a search network throw SearchException when a node cannot carry out the request or is lost.** Previously, they waited for ever. Catch [SearchException](https://reference.groupdocs.com/search/net/groupdocs.search.exceptions/searchexception/) around the calls of the Searcher and the Indexer of the network and around the configuration of the nodes; Indexer.GetAttributes of a document that the network has not indexed throws ArgumentException instead of KeyNotFoundException.
*   **The master node reports more errors by its ErrorOccurred event.** The documents and the shards that a node could not process, and the errors that the indexes of the nodes report, are raised by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/erroroccurred/) event of the master node; the operation completes with the other documents and shards.
*   **A node that sends nothing for 15 seconds is lost for the master node**, for example a slave node paused in a debugger. An update of a document whose shard is lost is reported by ErrorOccurred: index the document again when its node is connected again. [TcpSettings.ResponseTimeout](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/tcpsettings/responsetimeout/) limits the wait for the answer of a node; by default, the master node waits without a limit.
*   **A cancelled addition reports its documents.** Index again the documents that the ErrorOccurred event names as not indexed. Searcher.SearchNext does not observe the cancellation of the options of the first search.
*   **NodeEventHub.IndexingProgressChanged occurs once for every answer of a shard**, which may report several documents.
*   **Indexer.Synchronize does not start when the list of the documents of a shard cannot be read.** [Synchronize](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/synchronize/) throws InvalidOperationException before it sends anything to the shards.
*   **A configuration with a document type definition is refused.** [Configuration.Load](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.configuring/configuration/load/) throws XmlException for such a configuration: remove the DOCTYPE from a configuration written by hand.
*   **A disposed node throws ObjectDisposedException.** The calls of a disposed [SearchNetworkNode](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/searchnetworknode/), and a call that waits for the network while the node is disposed, throw ObjectDisposedException. An asynchronous operation of a disposed node ends without its completion event, and Dispose waits for the operations of the node.

See [Scaling]({{< ref "search/net/developer-guide/advanced-usage/scaling/_index.md" >}}).

#### Evaluation mode

*   **An enumeration of a search result throws after 10 documents.** A *foreach* loop or a LINQ query over a SearchResult or a NetworkSearchResult with more than 10 found documents throws [LicenseRestrictionException](https://reference.groupdocs.com/search/net/groupdocs.search.exceptions/licenserestrictionexception/) after the first 10 documents, as GetFoundDocument does for a number of 10 or greater. Enumerate at most 10 documents, for example with Take(10).
*   **Index.Update adds new files only within the limit** of 100 documents, as Index.Add does.
*   **Only the first 10 found images can be retrieved.** GetFoundImage of an ImageSearchResult or a NetworkImageSearchResult throws LicenseRestrictionException for a number of 10 or greater.

A licensed application is not affected. See [Evaluation limitations and licensing]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md" >}}).

#### Other changes

*   **Null arguments throw ArgumentNullException.** The operations of an index and of an index repository throw ArgumentNullException for an argument that is null, and ArgumentException for a document or extracted data that is null, before the operation starts; previously, they threw NullReferenceException or reported it by ErrorOccurred. [CustomExtractorCollection.Add](https://reference.groupdocs.com/search/net/groupdocs.search.common/customextractorcollection/add/) throws ArgumentNullException for an extractor that is null.
*   **License.SetLicense checks the subscription in the same way in every culture.** [SetLicense](https://reference.groupdocs.com/search/net/groupdocs.search/license/setlicense/) throws InvalidOperationException for a license whose subscription has ended before the release of the version also when the culture of the current thread has the Hijri or the Persian calendar.
*   **A finalizer of a class derived from ExtractionHost or Cancellation does not run after Dispose.** Previously, it ran when the object was collected.

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
