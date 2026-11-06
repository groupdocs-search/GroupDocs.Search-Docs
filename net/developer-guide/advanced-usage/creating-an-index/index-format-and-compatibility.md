---
id: index-format-and-compatibility
url: search/net/index-format-and-compatibility
title: Index format and compatibility
weight: 4
description: "This article describes the index format of GroupDocs.Search for .NET 26.10 and the compatibility rules for opening, updating, merging and saving indexes."
keywords: index format, index version, index compatibility, IndexUpdater, update index version, merge indexes, SaveTo
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page describes the index format introduced in GroupDocs.Search for .NET 26.10 and the rules that define which indexes can be opened, updated, merged and saved. Other pages refer to these rules:

*   [Migration notes]({{< ref "search/net/developer-guide/migration-notes.md" >}})
*   [Update index]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md" >}})
*   [Merge indexes]({{< ref "search/net/developer-guide/advanced-usage/indexing/merge-indexes.md" >}})
*   [Search index location]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-location.md" >}})

## Index format of version 26.10

Version 26.10 introduces a new format for storing an index on disk. Compared with previous versions, the new format takes less disk space and memory, is opened faster, protects index data with checksums and applies all changes to an index atomically: if an operation is interrupted by an error, a power failure or termination of the process, the index remains in its previous consistent state.

The new format determines the following compatibility rules:

*   Version 26.10 opens only indexes of the new format. The [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) constructors and the [LoadIntoMemoryCompletely](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) method throw [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) for an index created by a previous version, including versions 26.7 and 26.8. Such an index must be updated with the [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) class first.
*   Previous versions cannot open an index of version 26.10. Therefore, all applications that use the same index folder, including all nodes of a [search network]({{< ref "search/net/developer-guide/advanced-usage/scaling/_index.md" >}}), must be upgraded together.

## Indexes of version 26.11

Version 26.11 opens the indexes of version 26.10 without an update. However, an index created by version 26.11, or an index of version 26.10 that version 26.11 has changed (for example, by indexing, deletion of documents, optimization or a change of its settings), cannot be opened by version 26.10 and earlier versions. Therefore, all applications that use the same index folder, including all nodes of a search network, must be upgraded to version 26.11 together.

## Checking the version of an index

The [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) class provides two methods for checking an index folder before opening it:

*   The [IsLatestVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/islatestversion/) method returns *true* if the folder contains an index of the current version, which can be opened without updating.
*   The [CanUpdateVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/canupdateversion/) method returns *true* if the folder contains an index of a previous version that can be updated to the current version.

Both methods return *false* for a folder that does not contain an index and for an empty path.

## Opening an index

The [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) constructors and the [LoadIntoMemoryCompletely](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) method refuse an index that cannot be opened:

*   [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) is thrown for an index of a previous version that must be updated with the [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) class, for an index of a version that cannot be updated and, since version 26.11, for an index created or changed by a newer version of the library. An index of a newer version is not changed: no file of it is deleted, repaired or written over. Previously, such an index could be taken for a damaged one and refused with [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception).
*   [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception) is thrown for a damaged index: a file that the opening of the index reads is damaged. Since version 26.11, an index that has lost the files that record its changes, for example because they have been deleted, is also a damaged index; previously, it was refused with InvalidOperationException as an index of a version that cannot be updated. The folder of such an index is not changed; restore the index from a backup or create it again.
*   [FileNotFoundException](https://learn.microsoft.com/dotnet/api/system.io.filenotfoundexception) is thrown for a damaged index: a file that the opening of the index reads is missing.

Since version 26.11, the constructors also throw [IOException](https://learn.microsoft.com/dotnet/api/system.io.ioexception) if another Index object uses the folder (see [Search index location]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-location.md#index-folder" >}})).

Some errors that occur while an index is opened do not prevent its opening, for example a damaged dictionary of synonyms, which then gets its default content. Such errors occur before a handler of the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event can be added, so they are written to the [log]({{< ref "search/net/developer-guide/advanced-usage/indexing/logging.md" >}}) of the index ([IndexSettings.Logger](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/logger/)) and are listed in the [errors](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexingreport/errors/) of the next [indexing report]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-reports.md" >}}). Since version 26.11, this also applies to an index whose last operation could not be merged with it: the index is opened in its state before the operation, and the operation has to be repeated.

## Updating an index of a previous version

An index of a previous version is updated with the [UpdateVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/updateversion/) method. How the index is updated depends on the version that created it:

| Version that created the index | How the index is updated | Source documents required |
| --- | --- | --- |
| 22.7 – 26.8 | The index data are converted to the new format without re-indexing. | No |
| 18.5 – 22.6 | The documents are indexed again into a new index of the current version. | Yes, at the paths from which they were indexed |
| Earlier than 18.5 | The index cannot be updated; [UpdateVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/updateversion/) returns [VersionUpdateResult.Unsupported](https://reference.groupdocs.com/search/net/groupdocs.search.common/versionupdateresult/). Create a new index and add the documents to it. | Yes |

When an index is converted without re-indexing, the indexed documents, their attributes and stored texts, the image search data, the index settings and the dictionaries are preserved. Terms longer than 80 characters are not converted, because version 26.10 does not index such terms.

The [UpdateVersion](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/updateversion/) method follows these rules:

*   The source index is never modified. The updated index is written to the folder specified by the second parameter.
*   The folder for the updated index must differ from the folder of the source index: it must not be the source folder itself, a folder that contains it or a folder inside it, also when the folder is specified by another path (for example, through a link or a mapped drive). Otherwise, the method throws [ArgumentException](https://learn.microsoft.com/dotnet/api/system.argumentexception); for a *null* path, it throws [ArgumentNullException](https://learn.microsoft.com/dotnet/api/system.argumentnullexception).
*   If the folder for the updated index exists, its content is deleted before the updated index is written. If the update fails, the content may already be deleted, so do not specify a folder that contains files you need.
*   The method returns a value of the [VersionUpdateResult](https://reference.groupdocs.com/search/net/groupdocs.search.common/versionupdateresult/) enumeration:
    *   *Updated* — the index has been updated and written to the new folder;
    *   *AlreadyUpToDate* — the source folder already contains an index of the current version, nothing has been done;
    *   *Unsupported* — the source folder does not contain an index that can be updated.
*   The method throws [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception) if the source index is damaged and cannot be converted; in this case, nothing of the update remains in the new folder. It throws [IOException](https://learn.microsoft.com/dotnet/api/system.io.ioexception) or [UnauthorizedAccessException](https://learn.microsoft.com/dotnet/api/system.unauthorizedaccessexception) if a file or a folder cannot be accessed. Since version 26.11, it also throws IOException, and does not change the folder, if an [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) object uses the folder for the updated index.

After a successful update, the folder of the source index can be deleted.

The following example demonstrates how to open an index regardless of whether it was created by the current or a previous version.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\"; // The folder of an existing index
string updatedIndexFolder = @"c:\MyUpdatedIndex\"; // A new empty folder for the updated index

IndexUpdater updater = new IndexUpdater();
Index index;
if (updater.IsLatestVersion(indexFolder))
{
    // The index has the current version and can be opened as is
    index = new Index(indexFolder);
}
else if (updater.CanUpdateVersion(indexFolder))
{
    // The index of a previous version is converted or re-indexed into the new folder;
    // the source index is not changed
    VersionUpdateResult result = updater.UpdateVersion(indexFolder, updatedIndexFolder);
    if (result != VersionUpdateResult.Updated)
    {
        throw new InvalidOperationException("The index cannot be updated: " + result);
    }
    index = new Index(updatedIndexFolder);
}
else
{
    // The folder does not contain an index that can be opened or updated, so a new index is created;
    // the constructor refuses a folder with other content, for example a damaged index or an index of a newer version
    index = new Index(indexFolder);
}
```

## Settings of an updated index

Version 26.10 introduces two index settings that affect which terms are indexed: [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/). Both are enabled by default in a new index. Previous versions behaved as if both settings were disabled, so an index updated from a previous version has both settings disabled, and its documents remain consistent with their terms. See the [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}) page for details.

The settings that define how text is divided into terms are fixed when the index is created or once it contains documents:

*   The [IndexType](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/indextype/), [TextStorageSettings](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/textstoragesettings/), [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) settings are fixed when the index is created; a value assigned after that is ignored.
*   The [alphabet]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}), the [tokenization dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary.md" >}}), the [character replacements]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/character-replacements.md" >}}) and the [stop words]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary.md" >}}), as well as the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/) and [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/) settings, can be changed only while the index contains no documents and is not performing an operation. Otherwise, a change throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception); a call that does not change the content is allowed. Configure them once, after creating the index and before adding the first document.

The other dictionaries (synonyms, homophones, aliases, the spelling corrector and document passwords) can be changed at any time. The [SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/) setting and, since version 26.11, the [UseFastLoading](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usefastloading/) setting of an existing index can also be changed at any time through the [Index.IndexSettings](https://reference.groupdocs.com/search/net/groupdocs.search/index/indexsettings/) property; the new value is saved in the index.

## Merging compatibility

The [Merge](https://reference.groupdocs.com/search/net/groupdocs.search/index/merge/) method merges only indexes that divide the text of documents into terms in the same way, because a search finds a document reliably only if the document was indexed with the dictionaries and settings that the index uses. Before making any changes, the method compares the current index with every merged index that contains documents:

*   the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/), [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/), [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) settings;
*   the alphabet and the tokenization dictionary;
*   the stop word dictionary, if the indexes use stop words;
*   the character replacement dictionary, if the indexes use character replacements.

If a difference is found, the method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception), whose message names the differences, and no index is changed. This also happens when [MergeOptions.IsAsync](https://reference.groupdocs.com/search/net/groupdocs.search.options/mergeoptions/isasync/) is *true*.

Other rules of merging:

*   A merged index of a previous version must be updated with the [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) class before merging.
*   The alphabet, the tokenization dictionary, the stop word dictionary and the character replacement dictionary of the current index are not changed by merging. The other dictionaries of the merged indexes are merged into the dictionaries of the current index; since version 26.11, the synonyms and homophones of each word are kept as they are in the merged dictionaries.
*   Indexes of another [IndexType](https://reference.groupdocs.com/search/net/groupdocs.search.options/indextype/) are not merged.
*   A merge is not performed if a merged index is performing an operation or has failed when the merge starts; the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event reports it.
*   Since version 26.11, a merged index stays in its [index repository]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-repository.md" >}}); previously, the merge moved it out of the repository.

An index updated from a previous version has the [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) settings disabled, while a new index has them enabled by default. In addition, since version 26.11, the default [alphabet]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}) and [tokenization dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary.md" >}}) of a new index contain more scripts (the combining diacritical marks, Hebrew, Devanagari, Georgian, the extended Latin and Greek blocks and Hangul), while an index keeps the alphabet that it was created with. Therefore, a new index can merge an index of a previous version, also of version 26.10, only after it gets the alphabet and the tokenization dictionary of that index: export them from that index with the [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) method, then call Clear and [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) for each of them in the new index before adding documents to it, as shown on the [Alphabet]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}) page. The following example demonstrates how to create a new index into which an updated index can be merged.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string updatedIndexFolder = @"c:\MyUpdatedIndex\"; // An index updated from a previous version
string documentsFolder = @"c:\MyDocuments\";

Index updatedIndex = new Index(updatedIndexFolder);

// Exporting the alphabet and the tokenization dictionary of the updated index
updatedIndex.Dictionaries.Alphabet.ExportDictionary(@"c:\Alphabet.dat");
updatedIndex.Dictionaries.TokenizationDictionary.ExportDictionary(@"c:\TokenizationDictionary.dat");

// The new index divides text into terms as the previous versions did
IndexSettings settings = new IndexSettings();
settings.ReplaceUnpairedSurrogates = false;
settings.SkipBlendedOnlyTerms = false;

Index index = new Index(indexFolder, settings, true);

// Taking the dictionaries of the updated index while the new index contains no documents
index.Dictionaries.Alphabet.Clear();
index.Dictionaries.Alphabet.ImportDictionary(@"c:\Alphabet.dat");
index.Dictionaries.TokenizationDictionary.Clear();
index.Dictionaries.TokenizationDictionary.ImportDictionary(@"c:\TokenizationDictionary.dat");

index.Add(documentsFolder);

try
{
    index.Merge(updatedIndex, new MergeOptions());
}
catch (InvalidOperationException exception)
{
    // The indexes still divide text into terms differently, for example, they use different stop words;
    // the message names the differences
    Console.WriteLine(exception.Message);
}
```

## Saving an index to another folder

The [SaveTo](https://reference.groupdocs.com/search/net/groupdocs.search/index/saveto/) method saves an index, including an index loaded into memory, to another folder. The method follows these rules:

*   The method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if the folder is the folder of the index itself, also when it is specified differently (with a trailing separator, in a different letter case on Windows, or through a link or a mapped drive).
*   An index of the current version in the target folder is replaced by the saved index. Saving can be repeated to the same folder, also after a save that failed or was interrupted.
*   The method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) and does not change the folder if the folder contains an index of another version (for example, an index created by a previous version) or a damaged index. An application that regularly saves its index to the same folder, for example as a backup, has to empty that folder once after upgrading to version 26.10.
*   Since version 26.11, the method throws [IOException](https://learn.microsoft.com/dotnet/api/system.io.ioexception) and does not change the folder if another [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) object uses it.
*   Since version 26.11, the method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception) if the index is performing another operation; save the index when the operation has finished, for example after the [OperationFinished](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/operationfinished/) event. Previously, such an index was not saved, and only the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event reported it.
*   Since version 26.11, the method throws [InvalidDataException](https://learn.microsoft.com/dotnet/api/system.io.invaliddataexception) if a file of the index on disk is missing. This is found before the folder is changed, so an index that the folder contains stays as it was. Previously, the saved index lacked the file and did not work.
*   Since version 26.11, an index with the [Failed](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexstatus/) status can be saved: the saved index has the state that the index has worked with, without the change that has failed, and the index keeps its status. The settings changed after the failure are not saved.

## Search network

All nodes of a search network must use the same version of GroupDocs.Search. Before starting the nodes of version 26.10, update the index of each shard with the [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) class or create the shards anew. Since version 26.11, two nodes check the version of each other when they connect: a node of version 26.11 does not connect to a node of an earlier version, and the log of the node names the reason.

A shard accepts another content of the alphabet, the tokenization dictionary, the stop words or the character replacements from the [SetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/setdictionary/) method only while its index contains no documents. Otherwise, the shard keeps its own dictionary and writes the reason to its log, and since version 26.11, the SetDictionary method throws [SearchException](https://reference.groupdocs.com/search/net/groupdocs.search.exceptions/searchexception/); previously, it returned as if the dictionary was set. See the [Managing dictionaries in network]({{< ref "search/net/developer-guide/advanced-usage/scaling/managing-dictionaries-in-network.md" >}}) page for details.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
