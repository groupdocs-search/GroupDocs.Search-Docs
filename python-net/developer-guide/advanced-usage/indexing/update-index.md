---
id: update-index
url: search/python-net/update-index
title: Update index
weight: 26
description: "This article explains how to update indexed documents and how to update the version of an index created by a previous version."
keywords: update indexed documents, update index version, IndexUpdater, VersionUpdateResult, indexed_with_error, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains a description of updating indexed documents, as well as updating an index version.

## Update indexed documents

The update operation is used to reindex documents that have been changed, deleted or added to indexed folders. Changing a filter specified by the [document_filter](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/document_filter/) property of the [IndexSettings](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexsettings/) class can also lead to a change in the list of indexed documents.

The update operation also indexes again the documents whose previous indexing was not completed. For example, a document whose indexing was interrupted by [cancellation]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-options.md#cancellation-property" >}}) is marked with the [indexed_with_error](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/documentinfo/indexed_with_error/) flag and is indexed again by the next update. Note that only documents indexed from files can be indexed again: documents added from streams, structures or extracted data have to be added again. In the same way, the update indexes again the files of documents deleted when a damaged index is repaired (see [Checking index integrity]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}})).

When updating, the same options can be specified in the instance of the [UpdateOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/updateoptions/) class, which are set in the [IndexingOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/) class to specify indexing options. See the [Indexing options]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-options.md" >}}) page.

The following example demonstrates how to update an index using 2 threads.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index
from groupdocs.search.options import UpdateOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Change, delete, add documents in the document folder
# ...

options = UpdateOptions()
options.threads = 2  # Setting the number of indexing threads
index.update(options)  # Updating the index

print("Indexed documents:", len(index.get_indexed_documents()))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "update-indexed-documents.txt" >}}  
```text
Indexed documents: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/update-index/update_indexed_documents/update-indexed-documents.txt)
{{< /tab >}}
{{< /tabs >}}

## Update index version

When a new version of the GroupDocs.Search library changes the format for storing the index on disk, an index created by a previous version has to be updated to the new format with the [IndexUpdater](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/) class. Version 26.10 introduces a new index format: an index of a previous version, including versions 26.7 and 26.8, cannot be opened until its version is updated, and the [Index](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/) constructor raises `RuntimeError` for it.

The [update_version](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/update_version/) method updates an index as follows:

*   An index created by versions 22.7–26.8 is converted to the new format without re-indexing, so the source documents are not required.
*   An index created by versions 18.5–22.6 is updated by indexing the documents again, so the documents must be available at the paths from which they were indexed.
*   The source index is never modified. The updated index is written to another folder, which must not be the source folder, a folder that contains it or a folder inside it. The content of the target folder is deleted before the updated index is written.
*   The method returns a value of the [VersionUpdateResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/versionupdateresult/) enumeration: *UPDATED*, *ALREADY_UP_TO_DATE* (the index already has the current version) or *UNSUPPORTED* (the folder does not contain an index that can be updated).

The [is_latest_version](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/is_latest_version/) method checks whether a folder contains an index of the current version, and the [can_update_version](https://reference.groupdocs.com/search/python-net/groupdocs.search/indexupdater/can_update_version/) method checks whether a folder contains an index of a previous version that can be updated. After a successful update, the folder containing the old version of the index may be deleted. The full rules, including the exceptions and the settings of an updated index, are described on the [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

The following example demonstrates updating a previous version of an index.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexUpdater
from groupdocs.search.common import VersionUpdateResult

source_index_folder = "./old-index"
target_index_folder = "./new_index"

updater = IndexUpdater()

if updater.is_latest_version(source_index_folder):
    # The index has the current version and can be opened without updating
    index = Index(source_index_folder)
    print("The index has the current version")
elif updater.can_update_version(source_index_folder):
    # The index of the old version does not change
    result = updater.update_version(source_index_folder, target_index_folder)
    if result == VersionUpdateResult.UPDATED:
        # Opening the updated index
        index = Index(target_index_folder)
        print("The index is updated: " + str(len(index.get_indexed_documents())) + " documents")
```
{{< /tab >}}
{{< tab "old-index" >}}
{{< tab-text >}}
The following sample files are used in this example: [old-index/index.info](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/index.info), [old-index/index0.body](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/index0.body), [old-index/index0.head](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/index0.head), [old-index/index0.info](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/index0.info), [old-index/index1.info](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/index1.info), [old-index/indexAliases.term](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/indexAliases.term), [old-index/indexAlphabet.term](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/indexAlphabet.term), [old-index/indexCharacterReplacements.term](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/indexCharacterReplacements.term), [old-index/indexDocumentPasswords.dat](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/indexDocumentPasswords.dat), [old-index/indexHomophones.term](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/indexHomophones.term), [old-index/indexSpellingCorrector.dat](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/indexSpellingCorrector.dat), [old-index/indexStopWords.term](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/indexStopWords.term), [old-index/indexSynonyms.term](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/indexSynonyms.term), [old-index/tokenizationDictionary.term](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/update-index/old-index/tokenizationDictionary.term)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "update-index-version.txt" >}}  
```text
The index is updated: 4 documents
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/update-index/update_index_version/update-index-version.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
