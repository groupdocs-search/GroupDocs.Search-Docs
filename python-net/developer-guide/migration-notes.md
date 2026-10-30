---
id: migration-notes
url: search/python-net/migration-notes
title: Migration Notes
weight: 3
description: "This article describes what to take into account in GroupDocs.Search for Python via .NET 26.10 when you use indexes created by previous versions of GroupDocs.Search or port code written for them: the new index format, IndexUpdater and the behavior changes."
keywords: migration, upgrade, 26.10, index format, IndexUpdater, update index version, breaking changes, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
GroupDocs.Search for Python via .NET 26.10 is the first release of GroupDocs.Search for Python. It is built on version 26.10 of GroupDocs.Search, which introduces a new index format and changes in behavior. This page describes what to take into account if your Python application uses indexes created by previous versions of GroupDocs.Search, for example by an application built on GroupDocs.Search for .NET, or if you port code written for previous versions:

*   [Migrating to version 26.10](#migrating-to-version-2610) — the new index format and the behavior changes that may require changes in your code.

## Migrating to version 26.10

### What is new in the index format

Version 26.10 introduces a new format for storing an index on disk. Compared with previous versions:

*   the index size on disk grows in proportion to the indexed data, and a loaded index requires about half as much RAM;
*   an index is opened 2 to 4 times faster, and search in an index with many segments reads only the segments that contain the query terms;
*   index data are protected with checksums, and all changes to an index are applied atomically: if an operation is interrupted by an error, a power failure or termination of the process, the index remains in its previous consistent state;
*   deleting documents no longer rewrites the index, and searches and index-modifying operations no longer block each other.

The full rules of compatibility between the new format and indexes of previous versions are described on the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

### Opening indexes of previous versions

Version 26.10 opens only indexes of the new format. The [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) constructors and the [load_into_memory_completely](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/load_into_memory_completely/) method raise [RuntimeError](https://docs.python.org/3/library/exceptions.html#RuntimeError) for an index created by a previous version, including versions 26.7 and 26.8. Such an index must be updated with the [IndexUpdater](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/) class first:

*   An index created by versions 22.7–26.8 is converted to the new format without re-indexing; the source documents are not required.
*   An index created by versions 18.5–22.6 is re-indexed: the documents must be available at the paths from which they were indexed.
*   An index created by a version earlier than 18.5 cannot be updated; create a new index and add the documents to it.

The updated index is written to another folder; the source index is not modified. Conversely, previous versions cannot open an index of version 26.10.

The following example demonstrates how to update an index of a previous version before opening it.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexUpdater
from groupdocs.search.common import VersionUpdateResult

index_folder = "./old-index"  # The folder of an index created by a previous version
updated_index_folder = "./updated-index"  # A new empty folder for the updated index

updater = IndexUpdater()
if updater.can_update_version(index_folder):
    # The source index is not changed; the content of the target folder is deleted before the update
    result = updater.update_version(index_folder, updated_index_folder)
    if result == VersionUpdateResult.UPDATED:
        with Index(updated_index_folder) as index:
            search_result = index.search("Einstein")
            print("Documents found:", search_result.document_count)
```
{{< /tab >}}
{{< tab "old-index" >}}
{{< tab-text >}}
The following sample files are used in this example: [old-index/index.info](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/index.info), [old-index/index0.body](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/index0.body), [old-index/index0.head](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/index0.head), [old-index/index0.info](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/index0.info), [old-index/index1.info](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/index1.info), [old-index/indexAliases.term](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/indexAliases.term), [old-index/indexAlphabet.term](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/indexAlphabet.term), [old-index/indexCharacterReplacements.term](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/indexCharacterReplacements.term), [old-index/indexDocumentPasswords.dat](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/indexDocumentPasswords.dat), [old-index/indexHomophones.term](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/indexHomophones.term), [old-index/indexSpellingCorrector.dat](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/indexSpellingCorrector.dat), [old-index/indexStopWords.term](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/indexStopWords.term), [old-index/indexSynonyms.term](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/indexSynonyms.term), [old-index/tokenizationDictionary.term](/search/python-net/_sample_files/developer-guide/migration-notes/old-index/tokenizationDictionary.term)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "opening-indexes-of-previous-versions.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/migration-notes/opening_indexes_of_previous_versions/opening-indexes-of-previous-versions.txt)
{{< /tab >}}
{{< /tabs >}}

See [Update index version]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md#update-index-version" >}}) and [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) for the rules of the target folder, the values of [VersionUpdateResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/versionupdateresult/) and the exceptions.

### Upgrade checklist

*   **Upgrade all applications together.** All applications that use the same index folder, including applications built on GroupDocs.Search for .NET and all nodes of a [search network]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/_index.md" >}}), must be upgraded to version 26.10 at the same time. Update the index of each shard or create the shards anew before starting the nodes.
*   **Update indexes into new folders.** Update each index with [IndexUpdater](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/) into a new folder that is not the source folder, does not contain it and is not inside it; then switch the application to the new folder. See [Update index version]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md#update-index-version" >}}).
*   **Configure term dictionaries before adding documents.** The alphabet, the tokenization dictionary, the character replacements, the stop words and the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) and [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/) settings can be changed only while the index contains no documents. See [Settings of an updated index]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#settings-of-an-updated-index" >}}).
*   **Merge updated indexes into compatible indexes.** An index updated from a previous version has the [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) settings disabled. To merge it into a new index, create the new index with both settings set to *False*. See [Merging compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#merging-compatibility" >}}).
*   **Check the number of search threads.** Search now uses as many threads as there are processor cores by default. To restore single-threaded search with a deterministic order of results, set [search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/search_threads/) to *NumberOfThreads.ONE*. See [Search index settings]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}).
*   **Optimize the index to free disk space after deletions.** Deleted documents are excluded from search immediately but are physically removed only by [optimize](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/optimize/). See [Optimize index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}).
*   **Iterate over chunks by the token.** The number of chunks of a search by chunks is not known in advance; continue while [next_chunk_search_token](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/next_chunk_search_token/) is not *None*. See [Search by chunks]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}).
*   **Empty the backup folder of save_to once.** [save_to](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/save_to/) does not overwrite an index of a previous version. If an application regularly saves its index to the same folder, empty that folder once after the upgrade. See [Saving an index to another folder]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#saving-an-index-to-another-folder" >}}).
*   **Check documents interrupted by cancellation.** A document whose indexing was interrupted by cancellation is marked with [indexed_with_error](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/indexed_with_error/) and is indexed again by the next [update](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/update/). See [Indexing options]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-options.md" >}}).

### Breaking changes

The following changes in behavior compared with previous versions of GroupDocs.Search matter when you port code written for them or work with indexes they created.

#### Term dictionaries are fixed once an index contains documents

The [alphabet](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionaryrepository/alphabet/), [tokenization_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionaryrepository/tokenization_dictionary/), [character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionaryrepository/character_replacements/) and [stop_word_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionaryrepository/stop_word_dictionary/) dictionaries, as well as the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) and [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/) settings, can be changed only while the index contains no documents and is not performing an operation. Otherwise, a change raises [RuntimeError](https://docs.python.org/3/library/exceptions.html#RuntimeError); a call that does not change the content is allowed. Previously such a change was accepted, and documents indexed before and after it were processed by different rules. The other dictionaries (synonyms, homophones, aliases, the spelling corrector and document passwords) can be changed at any time.

Code that changes these dictionaries of an existing index must be moved to the place where the index is created. The following example demonstrates how to configure the dictionaries of a new index before adding documents.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.dictionaries import CharacterType

# Creating a new index; an existing index in the folder is overwritten
index = Index("./index", True)

# Configuring the term dictionaries while the index contains no documents
index.dictionaries.alphabet.set_range(["-"], CharacterType.BLENDED)
index.dictionaries.stop_word_dictionary.add_range(["and", "or", "the"])

# Indexing documents; after this, the dictionaries above cannot be changed
index.add("./documents")
print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "term-dictionaries-are-fixed-once-an-index-contains-documents.txt" >}}  
```text
Indexed documents: 4
```
[Download full output](/search/python-net/_output_files/developer-guide/migration-notes/term_dictionaries_are_fixed_once_an_index_contains_documents/term-dictionaries-are-fixed-once-an-index-contains-documents.txt)
{{< /tab >}}
{{< /tabs >}}

#### Incompatible indexes are not merged

The [merge](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/merge/) method raises [RuntimeError](https://docs.python.org/3/library/exceptions.html#RuntimeError) before making any changes if the merged indexes divide text into terms differently: they have different alphabets, tokenization dictionaries, stop words or character replacements, or different values of the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/), [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/), [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) or [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) settings. The exception is raised also when [MergeOptions.is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/mergeoptions/is_async/) is *True*. In addition, the term dictionaries of the current index are no longer changed by merging, and a merge is not performed if a merged index is performing an operation or has failed; the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event reports it.

In the following example, the other index uses an additional stop word, so the indexes divide text into terms differently.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import MergeOptions

index = Index("./index")
index.add("./documents")

other_index = Index("./other-index", True)
other_index.dictionaries.stop_word_dictionary.add_range(["physics"])
other_index.add("./documents")

try:
    index.merge(other_index, MergeOptions())
except RuntimeError as exception:
    # The indexes divide text into terms differently; the message names the differences
    print(exception)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "incompatible-indexes-are-not-merged.txt" >}}  
```text
The indexes cannot be merged because they divide the text of documents into terms differently. The index 'other-index' differs from the current index in: StopWordDictionary.
[.NET System.InvalidOperationException]
   at .(IndexRepository , MergeOptions ,  , OperationType )
   at .(IndexRepository , MergeOptions ,  , OperationType )
   at .(Index , MergeOptions )
   at GroupDocs.Search.Index.Merge(Index index, MergeOptions options)
   at InvokeStub_Index.Merge(Obje
[TRUNCATED]
```
[Download full output](/search/python-net/_output_files/developer-guide/migration-notes/incompatible_indexes_are_not_merged/incompatible-indexes-are-not-merged.txt)
{{< /tab >}}
{{< /tabs >}}

See [Merge indexes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/merge-indexes.md" >}}) and [Merging compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#merging-compatibility" >}}).

#### Search is multithreaded by default

With the default value of [IndexSettings.search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/search_threads/), search now uses as many threads as there are processor cores; previously only one thread was used. As a result, the order of the terms found by regular expression, numeric range, fuzzy search and spelling correction may differ from run to run, and so may the order of search results that depends on it. The property can be changed at any time through the [Index.index_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/index_settings/) property, and the new value is saved in the index; a value that is not a member of [NumberOfThreads](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/numberofthreads/) raises [ValueError](https://docs.python.org/3/library/exceptions.html#ValueError).

The following example demonstrates how to restore single-threaded search with a deterministic order of results.

{{< tabs "example4" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import NumberOfThreads

index = Index("./index")
index.add("./documents")
index.index_settings.search_threads = NumberOfThreads.ONE  # The value is saved in the index

result = index.search("^relativ.*")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-is-multithreaded-by-default.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/migration-notes/search_is_multithreaded_by_default/search-is-multithreaded-by-default.txt)
{{< /tab >}}
{{< /tabs >}}

#### New index settings change which terms are indexed

The new [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) settings are enabled by default in a new index and are fixed when the index is created: unpaired surrogate characters are replaced with the Unicode replacement character (U+FFFD) instead of being removed, and terms that consist only of blended characters are not indexed. To keep the behavior of previous versions, set both settings to *False* before creating an index, as shown on the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md#merging-compatibility" >}}) page. An index updated from a previous version has both settings disabled.

Other changes in text processing:

*   Terms longer than 80 characters are not indexed.
*   Letter case is converted in the same way on .NET Framework and .NET, so search results no longer depend on the platform.
*   The lookup methods of the stop word, synonym and homophone dictionaries ignore letter case; words are kept in lower case.

#### Search by chunks returns a different number of chunks

A chunk no longer corresponds to an index segment: only the segments that may contain matches are searched, and the number of chunks is not known in advance. Code that relies on the number of segments must iterate over the chunks while [next_chunk_search_token](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/searchresult/next_chunk_search_token/) is not *None*.

{{< tabs "example5" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import SearchOptions

index = Index("./index")
index.add("./documents")
options = SearchOptions()
options.is_chunk_search = True

result = index.search("Einstein", options)
document_count = result.document_count
while result.next_chunk_search_token is not None:
    result = index.search_next(result.next_chunk_search_token)
    document_count += result.document_count
print("Documents found:", document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/einstein.txt), [documents/nobel-prize-1921.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/nobel-prize-1921.txt), [documents/relativity.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/relativity.txt), [documents/time.txt](/search/python-net/_sample_files/developer-guide/migration-notes/documents/time.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "search-by-chunks-returns-a-different-number-of-chunks.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/migration-notes/search_by_chunks_returns_a_different_number_of_chunks/search-by-chunks-returns-a-different-number-of-chunks.txt)
{{< /tab >}}
{{< /tabs >}}

See [Search by chunks]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}).

#### Deleted documents are removed from disk by optimization

Deleting documents ([delete](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/delete/), or [update](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/update/) for deleted files) no longer rewrites the index. Deleted documents are excluded from search immediately, but their data remain on disk until the index is optimized with the [optimize](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/optimize/) method. Optimizing a very large index may leave several segments. See [Delete indexed documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/delete-indexed-documents.md" >}}) and [Optimize index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}).

#### Searches are not blocked by index modification

Searches and index-modifying operations (indexing, update, deletion, merging, optimization) no longer block each other. Each search works with a snapshot of the index taken when the search starts, so a search started during indexing does not find the documents being added. See [Search thread safety]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}}).

#### save_to does not overwrite an index of another version

The [save_to](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/save_to/) method raises [RuntimeError](https://docs.python.org/3/library/exceptions.html#RuntimeError) and does not change the target folder if the folder contains an index of another version or a damaged index, or if it is the folder of the index itself. An index of the current version in the target folder is replaced. An application that saves its index to the same folder, for example as a backup, has to empty that folder once after the upgrade.

#### Documents interrupted by cancellation are marked as indexed with error

If an indexing operation is cancelled, a document whose indexing was interrupted gets [DocumentInfo.indexed_with_error](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/indexed_with_error/) = *True* and is indexed again by the next [update](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/update/). Documents added from streams, structures or extracted data cannot be re-read by the update and have to be added again.
