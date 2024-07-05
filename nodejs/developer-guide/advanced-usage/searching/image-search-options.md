---
id: image-search-options
url: search/nodejs-java/image-search-options
title: Image search options
weight: 9.5
description: "This article describes the image search options that can be specified in an instance of the ImageSearchOptions class."
keywords: Image search options
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
This page contains a description of all search options that can be specified in an instance of the [ImageSearchOptions](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/ImageSearchOptions) class.

## setHashDifferences method

The [setHashDifferences](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/ImageSearchOptions#setHashDifferences(int)) method allows you to specify the maximum number of mismatched bits in the image hash. If all the bits in the hash match, then the images match completely. The more different bits, the more the images differ from each other. The default value is 5. The value must be in the range from 0 to 32.

## setMaxResultCount method

The [setMaxResultCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/ImageSearchOptions#setMaxResultCount(int)) method allows you to specify the maximum number of found images for an image reverse search request. The default value is 1000.

## setSearchDocumentFilter method

The [setSearchDocumentFilter](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.options/ImageSearchOptions#setSearchDocumentFilter(com.groupdocs.search.options.ISearchDocumentFilter)) method allows you to specify the filter of documents returned as a search result. By default, all documents found will be returned as a search result. For details, see the [Document filtering in search result]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
