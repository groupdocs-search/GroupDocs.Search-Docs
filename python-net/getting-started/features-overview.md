---
id: features-overview
url: search/python-net/features-overview
title: Features Overview
linkTitle: Features Overview
weight: 1
description: "GroupDocs.Search for Python via .NET creates, updates, merges and optimizes full-text indexes, and searches them with many search types: boolean, phrase, wildcard, regular expression, fuzzy, synonym, homophone, faceted, date and numeric range search, with highlighting, events and Python callbacks."
keywords: create index, update index, merge indexes, optimize index, index version update, search features, fuzzy search, highlight, events, python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
toc: True
---

GroupDocs.Search for Python via .NET indexes and searches documents of a wide range of [supported document formats]({{< ref "search/python-net/getting-started/supported-document-formats.md" >}}). Every task follows the same workflow: create or open an `Index`, `add()` documents to it, then `search()` it and read or highlight the results. The index lives in a folder of its own and is opened again on the next run, so documents are indexed once and searched many times. For details on a feature, follow the links to the [Developer Guide]({{< ref "search/python-net/developer-guide/_index.md" >}}).

## Creating and opening indexes

Create an index on disk or in memory, open it again on the next run, load an index into memory completely or save it to another folder, and keep several indexes in a repository that searches them all at once. Settings chosen at creation decide what an index keeps: a normal, compact or metadata-only index, with or without the text of the documents, stored at a choice of compression. An index created by a previous version of the library can be updated to the current format. See [Index lifecycle]({{< ref "search/python-net/developer-guide/basic-usage/index-lifecycle.md" >}}), [Search index location]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-location.md" >}}), [Search index settings]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}), [Search index repository]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-repository.md" >}}), and [Index format and compatibility]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}).

## Indexing documents

Index files, folders, streams and structures, including the documents inside ZIP archives and email attachments. Supply the passwords of protected documents, filter which documents are indexed, add fields of your own, assign attributes that can be changed without reindexing, and choose stop words, blended characters and character replacements. Text can be extracted separately from indexing, or in a separate process, and custom text extractors index formats the library does not know. See [Indexing from different sources]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-from-different-sources.md" >}}), [Indexing options]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-options.md" >}}), [Indexing password protected documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-password-protected-documents.md" >}}), [Document filtering during indexing]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}}), [Indexing additional fields]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-additional-fields.md" >}}), [Document attributes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-attributes.md" >}}), [Character types]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/character-types.md" >}}), [Separate data extraction]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/separate-data-extraction.md" >}}), and [Custom text extractors]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}).

## Keeping an index up to date

Update an index to take changed, deleted and added documents into account, merge several indexes into one, and optimize an index to speed up search and to remove deleted documents physically. Delete documents by path or by key, tell the index about renamed documents so they are not reindexed, and check an index's integrity and restore a damaged one. Index changes are atomic, and index data is protected by checksums. See [Update index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/update-index.md" >}}), [Merge indexes]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/merge-indexes.md" >}}), [Optimize index]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}), [Delete indexed documents]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/delete-indexed-documents.md" >}}), [Document renaming]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/document-renaming.md" >}}), and [Checking index integrity]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}}).

## Search types

Search for words, phrases and patterns with simple, boolean, wildcard, phrase (with wildcards) and regular-expression queries, and narrow a search to a field with faceted search. Date-range and numeric-range search find dates and numbers in the text, and queries can also target file names, document types, metadata fields and the creation and modification dates of files. Different search types combine in one query, written as text or built as objects. See [Build search query]({{< ref "search/python-net/developer-guide/basic-usage/build-search-query.md" >}}), [Boolean search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/boolean-search.md" >}}), [Phrase search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/phrase-search.md" >}}), [Wildcard search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/wildcard-search.md" >}}), [Regular expression search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/regular-expression-search.md" >}}), [Faceted search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/faceted-search.md" >}}), [Date range search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/date-range-search.md" >}}), [Numeric range search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/numeric-range-search.md" >}}), [Queries in text and object form]({{< ref "search/python-net/developer-guide/advanced-usage/searching/queries-in-text-and-object-form.md" >}}), and the [Query language specification]({{< ref "search/python-net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}).

## Finding what was meant

Fuzzy search finds words with typos and OCR noise, with a configurable similarity. Synonym, homophone and word-form search widen a query, case-sensitive search narrows it, aliases stand for longer queries, and spell checking and keyboard layout correction repair the query itself. See [Fuzzy search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/fuzzy-search.md" >}}), [Synonym search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/synonym-search.md" >}}), [Homophone search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/homophone-search.md" >}}), [Search for different word forms]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}), [Case sensitive search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/case-sensitive-search.md" >}}), [Using aliases]({{< ref "search/python-net/developer-guide/advanced-usage/searching/using-aliases.md" >}}), [Spell checking]({{< ref "search/python-net/developer-guide/advanced-usage/searching/spell-checking.md" >}}), and [Keyboard layout correction]({{< ref "search/python-net/developer-guide/advanced-usage/searching/keyboard-layout-correction.md" >}}).

## Search results and highlighting

A search result lists the found documents with their occurrences, relevance and found fields, and can be filtered by document attributes and other conditions. Highlight the found words in the text of a whole document or in short fragments, as HTML or plain text, read the text of any indexed document, and get reports of past searches. A large result can be read in chunks. See [Work with search results]({{< ref "search/python-net/developer-guide/basic-usage/work-with-search-results.md" >}}), [Document filtering in search result]({{< ref "search/python-net/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}), [Highlighting search results]({{< ref "search/python-net/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}), [Getting indexed documents]({{< ref "search/python-net/developer-guide/advanced-usage/searching/getting-indexed-documents.md" >}}), [Search reports]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-reports.md" >}}), and [Search by chunks]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}).

## Dictionaries

Each index keeps the dictionaries its search uses - synonyms, homophones, stop words, aliases, the alphabet, character replacements, the spelling corrector and document passwords - and each can be read, changed, exported and imported from Python. See [Managing Dictionaries]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}).

## Images and OCR

Calculate and index image hashes for reverse image search, which finds the documents that contain a picture like a given one. The text of images and scans is indexed through an OCR engine you connect. See [Reverse image search]({{< ref "search/python-net/developer-guide/advanced-usage/searching/reverse-image-search.md" >}}) and [OCR support]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/ocr-support.md" >}}).

## Events and Python callbacks

Index events report what an index does and let a handler take part: follow indexing progress, skip a document, or supply a password as a document needs it, with `index.events.file_indexing += handler`. Text extractors, OCR connectors, loggers and word form providers are plain Python classes or functions that the engine calls. See [Search index events]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}), [Logging]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/logging.md" >}}), [Indexing reports]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-reports.md" >}}), and [Word forms provider]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/word-forms-provider.md" >}}).

## Performance and concurrency

Searches are thread-safe and run while documents are indexed, updated, deleted, merged or optimized; one search can also use several threads. See [Search thread safety]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}}) and [Search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-options.md" >}}).

## Scaling

A search network distributes the documents over shards on several nodes and balances indexing and search across them. See [Scaling]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/_index.md" >}}).

## On-premise

GroupDocs.Search for Python via .NET runs entirely on your own infrastructure - your documents never leave your environment. The package is a self-contained wheel that bundles everything it needs, so no Microsoft Office, OpenOffice, or separate runtime has to be installed. See [System Requirements]({{< ref "search/python-net/getting-started/system-requirements.md" >}}) for the supported platforms and native dependencies.
