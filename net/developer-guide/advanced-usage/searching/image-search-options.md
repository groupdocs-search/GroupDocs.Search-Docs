---
id: image-search-options
url: search/net/image-search-options
title: Image search options
weight: 9.5
description: "This article describes the image search options that can be specified in an instance of the ImageSearchOptions class."
keywords: Image search options
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page contains a description of all search options that can be specified in an instance of the [ImageSearchOptions](https://apireference.groupdocs.com/net/search/groupdocs.search.options/imagesearchoptions) class.

## HashDifferences property

The [HashDifferences](https://apireference.groupdocs.com/net/search/groupdocs.search.options/imagesearchoptions/properties/hashdifferences) property allows you to specify the maximum number of mismatched bits in the image hash. If all the bits in the hash match, then the images match completely. The more different bits, the more the images differ from each other. The default value of the property is 5. The value must be in the range from 0 to 32.

## MaxResultCount property

The [MaxResultCount](https://apireference.groupdocs.com/net/search/groupdocs.search.options/imagesearchoptions/properties/maxresultcount) property allows you to specify the maximum number of found images for an image reverse search request. The default value is 1000.

## SearchDocumentFilter property

The [SearchDocumentFilter](https://apireference.groupdocs.com/net/search/groupdocs.search.options/imagesearchoptions/properties/searchdocumentfilter) property allows you to specify the filter of documents returned as a search result. By default, all documents found will be returned as a search result. For details, see the [Document filtering in search result]({{< ref "search/net/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
