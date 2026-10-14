---
id: features-overview
url: search/net/features-overview
title: Features Overview
weight: 2
description: "This article lists the indexing and search features of GroupDocs.Search for .NET: creating, updating, merging and optimizing indexes, many search types and much more."
keywords: create index, update index, merge indexes, optimize index, index version update, search features
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains a list of supported features. For detailed information on a particular feature, refer to the corresponding pages in the [Advanced Usage]({{< ref "search/net/developer-guide/advanced-usage/_index.md" >}}) section.

## Indexing

GroupDocs.Search provides the following indexing features:

*   Create index in memory or on disk.
*   Update index to take into account changed, deleted and added documents.
*   Merge several indexes into one.
*   Optimize index to improve search performance and to physically remove deleted documents.
*   Delete indexed documents by path or by document key.
*   Notify the index about renamed documents, so they are not reindexed.
*   Update the version of an index created by a previous version of the library.
*   Check index integrity and restore the functionality of a damaged index.
*   Atomic index changes and checksum protection of index data.
*   Load an index into memory completely and save an index to another folder.
*   Assign attributes to indexed documents and change them without reindexing.
*   Retrieve the text of indexed documents.
*   Indexing reports and notifications of index operations through events.
*   Ability of indexing from a file, stream or structure.
*   Indexing password protected documents.
*   Indexing with stop words.
*   Support for indexing additional fields.
*   Support for blended characters.
*   Support for characters indexed as a whole word.
*   Support for character replacement during indexing.
*   Support for custom text extractors.
*   Options for compact and metadata index.
*   Ability to save extracted text in index with different level of compression.
*   Document filtering during indexing.
*   Deleting indexed paths from index.
*   Ability to separately extract data from documents and index them.
*   Support for optical character recognition (OCR) on images through a connector to an OCR engine.
*   Calculation and indexing of image hashes for reverse image search.
*   The ability to create a distributed search network that automatically balances the load across nodes.

## Searching

GroupDocs.Search provides the following search features:

*   Simple word search.
*   Boolean search.
*   Regular expression search.
*   Faceted search.
*   Case sensitive search.
*   Flexible fuzzy search.
*   Synonym search.
*   Homophone search.
*   Wildcard search.
*   Phrase search with wildcards.
*   Search for different word forms.
*   Date range search.
*   Numeric range search.
*   Search by chunks (pages).
*   Document filtering in search result.
*   Search for different object types: text, numbers, dates, file names, document types, metadata fields, document creation/modification dates.
*   Combining different types of search into one search query.
*   Alias substitution in search queries.
*   Perform spell check during search.
*   Perform keyboard layout correction during search.
*   Search queries in text or flexible object form.
*   Highlighting search results in the text of the entire document or in text segments.
*   Multiple simultaneous thread safe search.
*   Multithreaded search with a configurable number of threads.
*   Searches are not blocked by indexing, updating, deleting, merging or optimizing operations.
*   Filtering search results by document attributes.
*   Search reports.
*   Search over several indexes simultaneously.
*   Built-in support for reverse image search.
