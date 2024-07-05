---
id: search-options
url: search/nodejs-java/search-options
title: Search options
weight: 22
description: "This article describes the search options that can be specified in an instance of the SearchOptions class."
keywords: Search options
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page contains a description of all search options that can be specified in an instance of the [SearchOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions) class.

## setCancellation method

The [setCancellation](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setCancellation(com.groupdocs.search.common.Cancellation)) method allows you to specify the object to cancel the search operation. The search can be canceled without delay by calling the [Cancel](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/Cancellation#cancel()) method or by calling the [CancelAfter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.common/Cancellation#cancelAfter(int)) method with the delay in milliseconds as an argument.

## getDateFormats method

The [getDateFormats](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#getDateFormats()) method returns a collection of date formats that allows you to specify formats for the date range search feature. For details, see the [Date range search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/date-range-search.md" >}}) page.

## getFuzzySearch method

The [getFuzzySearch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#getFuzzySearch()) method returns a fuzzy search options object that allows you to enable fuzzy search and set its options. By default, the fuzzy search feature is disabled. For details, see the [Fuzzy search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/fuzzy-search.md" >}}) page.

## setChunkSearch method

The [setChunkSearch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setChunkSearch(boolean)) method allows you to enable search by chunks. By default, the search by chunks feature is disabled. For details, see the [Search by chunks]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) page.

## getKeyboardLayoutCorrector method

The [getKeyboardLayoutCorrector](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#getKeyboardLayoutCorrector()) method returns a keyboard layout corrector options object, which allows you to enable keyboard layout correction in search queries. By default, the keyboard layout correction feature is disabled. See the [Keyboard layout correction]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/keyboard-layout-correction.md" >}}) page for details.

## setMaxOccurrenceCountPerTerm method

The [setMaxOccurrenceCountPerTerm](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setMaxOccurrenceCountPerTerm(int)) method allows you to specify the maximum number of occurrences of each word found that will be returned as a search result. The default value is 100000.

## setMaxTotalOccurrenceCount method

The [setMaxTotalOccurrenceCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setMaxTotalOccurrenceCount(int)) method allows you to specify the maximum number of occurrences in total for all found words, which will be returned as a search result. The default value is 500000.

## setSearchDocumentFilter method

The [setSearchDocumentFilter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setSearchDocumentFilter(com.groupdocs.search.options.ISearchDocumentFilter)) method allows you to specify the filter of documents returned as a search result. By default, all documents found will be returned as a search result. For details, see the [Document filtering in search result]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}) page.

## getSpellingCorrector method

The [getSpellingCorrector](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#getSpellingCorrector()) method returns the spelling corrector options object, which allows you to enable spelling correction in search queries and specify spelling correction options. By default, spelling correction is disabled. See the [Spell checking]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/spell-checking.md" >}}) page for details.

## setUseCaseSensitiveSearch method

The [setUseCaseSensitiveSearch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setUseCaseSensitiveSearch(boolean)) method allows you to perform case-sensitive search. Please note that case-sensitive search is not compatible with other types of searches. By default, case-insensitive search is performed. For details, see the [Case sensitive search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/case-sensitive-search.md" >}}) page.

## setUseHomophoneSearch method

The [setUseHomophoneSearch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setUseHomophoneSearch(boolean)) method allows you to enable homophone search feature. By default, the homophone search feature is disabled. For details, see the [Homophone search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/homophone-search.md" >}}) page.

## setUseSynonymSearch method

The [setUseSynonymSearch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setUseSynonymSearch(boolean)) method allows you to enable synonym search feature. By default, the synonym search feature is disabled. For details, see the [Synonym search]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/synonym-search.md" >}}) page.

## setUseWordFormsSearch method

The [setUseWordFormsSearch](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/SearchOptions#setUseWordFormsSearch(boolean)) method allows you to enable the word forms search feature. By default, the word forms search feature is disabled. For details, see the [Search for different word forms]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
