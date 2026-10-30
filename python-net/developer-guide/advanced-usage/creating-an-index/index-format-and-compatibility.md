---
id: index-format-and-compatibility
url: search/python-net/index-format-and-compatibility
title: Index format and compatibility
weight: 4
description: "This article describes the index format of GroupDocs.Search for Python via .NET 26.10 and the compatibility rules for opening, updating, merging and saving indexes."
keywords: index format, index version, index compatibility, IndexUpdater, update index version, merge indexes, save_to
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page describes the index format introduced in GroupDocs.Search 26.10 and the rules that define which indexes can be opened, updated, merged and saved. Other pages refer to these rules:

*   [Migration notes]({{< ref "search/python-net/developer-guide/migration-notes.md" >}})
*   [Update index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md" >}})
*   [Merge indexes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/merge-indexes.md" >}})
*   [Search index location]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-location.md" >}})

## Index format of version 26.10

Version 26.10 introduces a new format for storing an index on disk. Compared with previous versions, the new format takes less disk space and memory, is opened faster, protects index data with checksums and applies all changes to an index atomically: if an operation is interrupted by an error, a power failure or termination of the process, the index remains in its previous consistent state.

The new format determines the following compatibility rules:

*   Version 26.10 opens only indexes of the new format. The [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) constructors and the [load_into_memory_completely](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/load_into_memory_completely/) method raise `RuntimeError` for an index created by a previous version, including versions 26.7 and 26.8. Such an index must be updated with the [IndexUpdater](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/) class first.
*   Previous versions cannot open an index of version 26.10. Therefore, all applications that use the same index folder, including all nodes of a [search network]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/_index.md" >}}), must be upgraded together.

## Checking the version of an index

The [IndexUpdater](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/) class provides two methods for checking an index folder before opening it:

*   The [is_latest_version](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/is_latest_version/) method returns `True` if the folder contains an index of the current version, which can be opened without updating.
*   The [can_update_version](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/can_update_version/) method returns `True` if the folder contains an index of a previous version that can be updated to the current version.

Both methods return `False` for a folder that does not contain an index and for an empty path.

## Updating an index of a previous version

An index of a previous version is updated with the [update_version](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/update_version/) method. How the index is updated depends on the version that created it:

| Version that created the index | How the index is updated | Source documents required |
| --- | --- | --- |
| 22.7 – 26.8 | The index data are converted to the new format without re-indexing. | No |
| 18.5 – 22.6 | The documents are indexed again into a new index of the current version. | Yes, at the paths from which they were indexed |
| Earlier than 18.5 | The index cannot be updated; [update_version](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/update_version/) returns [VersionUpdateResult.UNSUPPORTED](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/versionupdateresult/). Create a new index and add the documents to it. | Yes |

When an index is converted without re-indexing, the indexed documents, their attributes and stored texts, the image search data, the index settings and the dictionaries are preserved. Terms longer than 80 characters are not converted, because version 26.10 does not index such terms.

The [update_version](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/update_version/) method follows these rules:

*   The source index is never modified. The updated index is written to the folder specified by the second parameter.
*   The folder for the updated index must differ from the folder of the source index: it must not be the source folder itself, a folder that contains it or a folder inside it, also when the folder is specified by another path (for example, through a link or a mapped drive). Otherwise, the method raises `ValueError`; for a `None` path, it also raises `ValueError`.
*   If the folder for the updated index exists, its content is deleted before the updated index is written. If the update fails, the content may already be deleted, so do not specify a folder that contains files you need.
*   The method returns a value of the [VersionUpdateResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/versionupdateresult/) enumeration:
    *   `UPDATED` — the index has been updated and written to the new folder;
    *   `ALREADY_UP_TO_DATE` — the source folder already contains an index of the current version, nothing has been done;
    *   `UNSUPPORTED` — the source folder does not contain an index that can be updated.
*   The method raises [GroupDocsSearchException](https://reference.groupdocs.com/search/python-net/groupdocs.search/groupdocssearchexception/) if the source index is damaged and cannot be converted; in this case, nothing of the update remains in the new folder. It raises `PermissionError` or [GroupDocsSearchException](https://reference.groupdocs.com/search/python-net/groupdocs.search/groupdocssearchexception/) if a file or a folder cannot be accessed.

After a successful update, the folder of the source index can be deleted.

The following example demonstrates how to open an index regardless of whether it was created by the current or a previous version. To print the name of a result, the example passes it to `VersionUpdateResult`.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexUpdater
from groupdocs.search.common import VersionUpdateResult

index_folder = "./old-index"  # The folder of an existing index, here one created by a previous version
updated_index_folder = "./updated-index"  # A new empty folder for the updated index

updater = IndexUpdater()
if updater.is_latest_version(index_folder):
    # The index has the current version and can be opened as is
    index = Index(index_folder)
    print("The index has the current version.")
elif updater.can_update_version(index_folder):
    # The index of a previous version is converted or re-indexed into the new folder;
    # the source index is not changed
    result = updater.update_version(index_folder, updated_index_folder)
    if result != VersionUpdateResult.UPDATED:
        raise RuntimeError("The index cannot be updated: " + result.name)
    index = Index(updated_index_folder)
    print("The index has been updated.")
else:
    # The folder does not contain an index that can be opened or updated, so a new index is created
    index = Index(index_folder, True)
    print("A new index has been created.")

print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "old-index" >}}
{{< tab-text >}}
The following sample files are used in this example: [old-index/index.info](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/index.info), [old-index/index0.body](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/index0.body), [old-index/index0.head](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/index0.head), [old-index/index0.info](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/index0.info), [old-index/index1.info](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/index1.info), [old-index/indexAliases.term](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/indexAliases.term), [old-index/indexAlphabet.term](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/indexAlphabet.term), [old-index/indexCharacterReplacements.term](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/indexCharacterReplacements.term), [old-index/indexDocumentPasswords.dat](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/indexDocumentPasswords.dat), [old-index/indexHomophones.term](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/indexHomophones.term), [old-index/indexSpellingCorrector.dat](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/indexSpellingCorrector.dat), [old-index/indexStopWords.term](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/indexStopWords.term), [old-index/indexSynonyms.term](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/indexSynonyms.term), [old-index/tokenizationDictionary.term](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/old-index/tokenizationDictionary.term)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "updating-an-index-of-a-previous-version.txt" >}}  
```text
The index has been updated.
Indexed documents: 4
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/updating_an_index_of_a_previous_version/updating-an-index-of-a-previous-version.txt)
{{< /tab >}}
{{< /tabs >}}

## Settings of an updated index

Version 26.10 introduces two index settings that affect which terms are indexed: [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/). Both are enabled by default in a new index. Previous versions behaved as if both settings were disabled, so an index updated from a previous version has both settings disabled, and its documents remain consistent with their terms. See the [Search index settings]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}) page for details.

The settings that define how text is divided into terms are fixed when the index is created or once it contains documents:

*   The [index_type](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/index_type/), [text_storage_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/text_storage_settings/), [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) settings are fixed when the index is created; a value assigned after that is ignored.
*   The [alphabet]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/alphabet.md" >}}), the [tokenization dictionary]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary.md" >}}), the [character replacements]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/character-replacements.md" >}}) and the [stop words]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/stop-word-dictionary.md" >}}), as well as the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/) and [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/) settings, can be changed only while the index contains no documents and is not performing an operation. Otherwise, a change raises `RuntimeError`; a call that does not change the content is allowed. Configure them once, after creating the index and before adding the first document.

The other dictionaries (synonyms, homophones, aliases, the spelling corrector and document passwords) can be changed at any time. The [search_threads](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/search_threads/) setting of an existing index can also be changed at any time through the [Index.index_settings](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/index_settings/) property; the new value is saved in the index.

## Merging compatibility

The [merge](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/merge/) method merges only indexes that divide the text of documents into terms in the same way, because a search finds a document reliably only if the document was indexed with the dictionaries and settings that the index uses. Before making any changes, the method compares the current index with every merged index that contains documents:

*   the [use_stop_words](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_stop_words/), [use_character_replacements](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/use_character_replacements/), [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) settings;
*   the alphabet and the tokenization dictionary;
*   the stop word dictionary, if the indexes use stop words;
*   the character replacement dictionary, if the indexes use character replacements.

If a difference is found, the method raises `RuntimeError`, whose message names the differences, and no index is changed. This also happens when [MergeOptions.is_async](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/mergeoptions/is_async/) is `True`.

Other rules of merging:

*   A merged index of a previous version must be updated with the [IndexUpdater](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/) class before merging.
*   The alphabet, the tokenization dictionary, the stop word dictionary and the character replacement dictionary of the current index are not changed by merging. The other dictionaries of the merged indexes are merged into the dictionaries of the current index.
*   Indexes of another [IndexType](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indextype/) are not merged.
*   A merge is not performed if a merged index is performing an operation or has failed when the merge starts; the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event reports it.

An index updated from a previous version has the [replace_unpaired_surrogates](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/replace_unpaired_surrogates/) and [skip_blended_only_terms](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/skip_blended_only_terms/) settings disabled, while a new index has them enabled by default. The following example demonstrates how to create a new index into which an updated index can be merged. The example prints the exception's `message` attribute, the message alone; `str(exception)` also contains the .NET stack trace.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings
from groupdocs.search.options import MergeOptions

# The new index divides text into terms as the previous versions did
settings = IndexSettings()
settings.replace_unpaired_surrogates = False
settings.skip_blended_only_terms = False

index = Index("./index", settings, True)
index.add("./documents")

updated_index = Index("./updated-index")  # An index updated from a previous version
try:
    index.merge(updated_index, MergeOptions())
    print("Indexed documents after merging:", len(index.get_indexed_documents()))
except RuntimeError as exception:
    # The indexes divide text into terms differently, for example, they use different alphabets;
    # the message names the differences
    print(exception.message)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-notes.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/documents/einstein-notes.txt), [documents/newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/documents/newton.txt), [documents/curie.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/documents/curie.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "merging-compatibility.txt" >}}  
```text
Indexed documents after merging: 7
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility/merging_compatibility/merging-compatibility.txt)
{{< /tab >}}
{{< /tabs >}}

## Saving an index to another folder

The [save_to](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/save_to/) method saves an index, including an index loaded into memory, to another folder. The method follows these rules:

*   The method raises `RuntimeError` if the folder is the folder of the index itself, also when it is specified differently (with a trailing separator, in a different letter case on Windows, or through a link or a mapped drive).
*   An index of the current version in the target folder is replaced by the saved index. Saving can be repeated to the same folder, also after a save that failed or was interrupted.
*   The method raises `RuntimeError` and does not change the folder if the folder contains an index of another version (for example, an index created by a previous version) or a damaged index. An application that regularly saves its index to the same folder, for example as a backup, has to empty that folder once after upgrading to version 26.10.

## Search network

All nodes of a search network must use the same version of GroupDocs.Search. Before starting the nodes of version 26.10, update the index of each shard with the [IndexUpdater](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/) class or create the shards anew.

A shard accepts another content of the alphabet, the tokenization dictionary, the stop words or the character replacements from the [set_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.scaling/indexer/set_dictionary/) method only while its index contains no documents. Otherwise, the shard keeps its own dictionary and writes the reason to its log. See the [Managing dictionaries in network]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/managing-dictionaries-in-network.md" >}}) page for details.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
