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
*   Reduce the disk space of an index at the cost of its loading speed.
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
*   The ability to create a distributed search network that automatically balances the load across nodes and authenticates them with a network key.

## Searching

GroupDocs.Search provides the following search features:

*   Simple word search.
*   Boolean search.
*   Regular expression search.
*   Faceted search.
*   Case sensitive search.
*   Flexible fuzzy search.
*   Synonym search.
*   Semantic expansion of queries: search for the words that are close in meaning to the words of a query, by a word vectors model.
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
*   Highlighting search results in the text of the entire document or in text segments, also for documents indexed with a custom word splitter.
*   Multiple simultaneous thread safe search.
*   Multithreaded search with a configurable number of threads.
*   Searches are not blocked by indexing, updating, deleting, merging or optimizing operations.
*   Filtering search results by document attributes.
*   Search reports.
*   Search over several indexes simultaneously, also by chunks.
*   Built-in support for reverse image search.
*   Ordering of the found documents by relevance (Okapi BM25).

## AI search

Since version 26.12, GroupDocs.Search provides the following features of the [AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/_index.md" >}}):

*   Ranked search by a query in a natural language, which returns the most relevant documents first with the best passages of their text.
*   Search by meaning with neural terms: a model is needed only to encode the documents, not to search.
*   Search by meaning and between languages with passage vectors: by a built-in model that needs no neural network, by a neural model or by a model of a service.
*   Merging of the search by words and the search by meaning in one result (hybrid search).
*   Reranking of the best passages by a model that reads the query together with a passage.
*   Local models: the text of the documents does not leave the computer.
*   Answers to questions by the documents of an index, in which every statement is confirmed by a quote found in a document.
*   Translation of requests in a natural language into queries of the index.
*   Sorting of documents into categories that are described in words, with or without a language model.
*   Suggestion of synonyms for the synonym dictionary.
*   Connection of language models and embedding services through Microsoft.Extensions.AI.
