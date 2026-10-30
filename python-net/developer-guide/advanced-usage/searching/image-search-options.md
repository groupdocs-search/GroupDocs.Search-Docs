---
id: image-search-options
url: search/python-net/image-search-options
title: Image search options
weight: 9.5
description: "This article describes the image search options that can be specified in an instance of the ImageSearchOptions class."
keywords: Image search options
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains a description of all search options that can be specified in an instance of the [ImageSearchOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imagesearchoptions/) class.

## hash_differences property

The [hash_differences](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imagesearchoptions/hash_differences/) property allows you to specify the maximum number of mismatched bits in the image hash. If all the bits in the hash match, then the images match completely. The more different bits, the more the images differ from each other. The default value of the property is 5. Since the image hash is 64 bits long, the meaningful values are in the range from 0 to 64: a value greater than 64 is treated as 64, and a negative value means that no images are found.

## max_result_count property

The [max_result_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imagesearchoptions/max_result_count/) property allows you to specify the maximum number of found images for a reverse image search request. The found images with fewer mismatched bits are returned first. The default value is 1000.

## search_document_filter property

The [search_document_filter](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/imagesearchoptions/search_document_filter/) property allows you to specify the filter of documents returned as a search result. By default, all documents found will be returned as a search result. For details, see the [Document filtering in search result]({{< ref "search/python-net/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
