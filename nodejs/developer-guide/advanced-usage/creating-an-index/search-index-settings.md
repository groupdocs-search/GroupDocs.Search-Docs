---
id: search-index-settings
url: search/nodejs-java/search-index-settings
title: Search index settings
weight: 3
description: "This page contains a description of all index settings that can be specified in an instance of the IndexSettings class."
keywords: index settings, description of all index settings
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page contains a description of all index settings that can be specified in an instance of the [IndexSettings](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings) class.

## setAutoDetectEncoding method

The [setAutoDetectEncoding](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setAutoDetectEncoding(boolean)) method sets a flag that allows you to automatically detect the following encodings of text files during indexing: UTF-32 LE, UTF-32 BE, UTF-16 LE, UTF-16 BE, UTF-8, UTF-7, ANSI. By default, the encoding auto detection of text files is disabled. But in any case, the encoding of a text file can be set during indexing when the [FileIndexing](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.events/EventHub#FileIndexing) event is raised. Detailed information on detecting and setting the encoding of text files is presented on the page [Text file encoding detection]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/text-file-encoding-detection.md" >}}).

## getCustomExtractors method

The [getCustomExtractors](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#getCustomExtractors()) method returns a collection of custom text extractors that allows adding new extractors for supported or not supported formats. A complete example of implementing a custom text extractor and using it to extract text is presented on the page [Custom text extractors]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}).

## setDocumentFilter method

The [setDocumentFilter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setDocumentFilter(com.groupdocs.search.DocumentFilter)) method allows you to set a filter used to determine whether files adding to the index should be indexed. If a document adding separately or located in the adding folder does not match the filter, then it will not be added and indexed. The default value is null, which means that all added files will be indexed if their format is supported. Detailed information on creating and setting the document indexing filter can be found on the page [Document filtering during indexing]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/document-filtering-during-indexing.md" >}}).

## setIndexType method

The [setIndexType](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setIndexType(int)) method is used to specify the type of an index. The [IndexType](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/IndexType) class contains 3 values:

*   [NormalIndex](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/IndexType#NormalIndex) - the type of an index containing the content and metadata of the added documents. It supports all search features.
*   [MetadataIndex](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/IndexType#MetadataIndex) - the type of an index containing only metadata of added documents. It supports all search features. An example of creating an index of this type is presented on the [Indexing metadata of documents]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/indexing-metadata-of-documents.md" >}}) page.
*   [CompactIndex](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/IndexType#CompactIndex) - the type of an index containing the content and metadata of the added documents. It takes much less disk space, but does not support the [phrase search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/phrase-search.md" >}}) and the [date range search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/date-range-search.md" >}}) features.

The default value is [NormalIndex](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/IndexType#NormalIndex).

## getInMemoryIndex method

The [getInMemoryIndex](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#getInMemoryIndex()) method returns a value indicating whether an index is located in RAM or on disk. The return value for this method is set indirectly when creating or loading an index. An index is created on disk, when specifying the path to the index folder, otherwise an index is created in memory.

## setLogger method

The [setLogger](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setLogger(com.groupdocs.search.common.ILogger)) method allows you to specify in the index settings the logger used for logging index events and errors during its operation. The default value is null meaning that logging is not used. Detailed information on creating and assigning an index logger and the implementation of a custom logger is presented on the page [Logging]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/logging.md" >}}).

## setMaxIndexingReportCount method

The [setMaxIndexingReportCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setMaxIndexingReportCount(int)) method allows you to specify the maximum number of indexing reports stored in RAM for an index since it was created or loaded. The default value is 5. Detailed information on indexing reports is provided on the page [Indexing reports]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/indexing-reports.md" >}}).

## setMaxSearchReportCount method

The [setMaxSearchReportCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setMaxSearchReportCount(int)) method allows you to specify the maximum number of search reports stored in RAM for an index since it was created or loaded. The default value is 10. Detailed information on search reports is provided on the page [Search reports]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/search-reports.md" >}}).

## setSearchThreads method

The [setSearchThreads](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setSearchThreads(int)) method allows you to set the number of threads used to search in an index. By default, this value is [NumberOfThreads.Default](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/NumberOfThreads#Default), which means that the search will be performed using the number of threads equal to the number of processor cores. This number of threads ensures the optimal rate of each individual search in an index. If you plan a large number of parallel search queries and you need to ensure maximum total search performance, you should set the [NumberOfThreads.One](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/NumberOfThreads#One) value for this parameter.

## setTextStorageSettings method

The [setTextStorageSettings](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setTextStorageSettings(com.groupdocs.search.options.TextStorageSettings)) method allows you to specify the settings for saving the text of indexed documents in an index. The default value is null, which means that document texts are not stored. Detailed information on saving the text of indexed documents in an index is presented on the page [Storing text of indexed documents]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}).

## setUseCharacterReplacements method

The [setUseCharacterReplacements](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setUseCharacterReplacements(boolean)) method allows you to set the value indicating whether to perform character replacements during the indexing process or not. The default value is false. Details on replacing characters during indexing are provided on the page [Character replacement during Indexing]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/character-replacement-during-indexing.md" >}}).

## setUseStopWords method

The [setUseStopWords](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setUseStopWords(boolean)) method allows you to specify a value indicating whether to use stop words during indexing or not. Stop words are frequently used words that do not carry a semantic load, which are removed from an index to reduce its size. The default value for this parameter is true. Detailed information on stop words and their use is presented on the page [Indexing with stop words]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/indexing-with-stop-words.md" >}}).

## setUseRawTextExtraction method

Property [setUseRawTextExtraction](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/IndexSettings#setUseRawTextExtraction(boolean)) allows you to specify a value indicating whether to use raw text extraction  mode whenever possible during indexing. The raw text extraction mode can significantly speed up the indexing process at the cost of losing the  formatting quality of the extracted text. The default value for this  parameter is true.



## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
