---
id: merge-indexes
url: search/net/merge-indexes
title: Merge indexes
weight: 20
description: "This article demonstrates the merge operation which is designed to combine two or more indexes into one index to accelerate the search and to simplify the work with indexes."
keywords: merge indexes, merge operation, index repository, MergeOptions
productName: GroupDocs.Search for .NET
hideChildren: False
---
The merge operation is designed to combine two or more indexes into one index to accelerate the search and to simplify the work with indexes. When merging, only the index at which the [Merge](https://reference.groupdocs.com/search/net/groupdocs.search/index/merge/) method was called is changed. This index as a result of the operation contains all the documents that were contained in all indexes together. The second index or index repository after the merge can be deleted to free up disk space.

The [MergeOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/mergeoptions/) class contains a property for specifying a cancellation object, as well as a property for specifying whether the operation will be performed asynchronously. By default, index merging operation is performed synchronously.

The [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class has method overloads for merging a single index and a whole index repository. Before performing a merge, checks are made to ensure that the indexes are of the same type. Merging is performed only on those indexes that have the same [IndexType](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/indextype/) value as the index on which the [Merge](https://reference.groupdocs.com/search/net/groupdocs.search/index/merge/) method is called.

## Merging compatibility

Since version 26.10, only indexes that divide the text of documents into terms in the same way can be merged:

*   Before making any changes, the [Merge](https://reference.groupdocs.com/search/net/groupdocs.search/index/merge/) method compares the current index with every merged index that contains documents: the [UseStopWords](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usestopwords/), [UseCharacterReplacements](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/usecharacterreplacements/), [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) settings, the alphabet, the tokenization dictionary, and also the stop words and the character replacements if the indexes use them. If a difference is found, the method throws [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception), whose message names the differences, and no index is changed. This also happens in the asynchronous mode.
*   The alphabet, the tokenization dictionary, the stop word dictionary and the character replacement dictionary of the current index are not changed by merging. The other dictionaries of the merged indexes (for example, synonyms and homophones) are merged into the dictionaries of the current index.
*   A merged index created by a previous version must be updated with the [IndexUpdater](https://reference.groupdocs.com/search/net/groupdocs.search/indexupdater/) class before merging (see [Update index]({{< ref "search/net/developer-guide/advanced-usage/indexing/update-index.md#update-index-version" >}})).
*   A merge is not performed if a merged index is performing an operation or has failed; the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event reports it.

Note that an index updated from a previous version has the [ReplaceUnpairedSurrogates](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/replaceunpairedsurrogates/) and [SkipBlendedOnlyTerms](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/skipblendedonlyterms/) settings disabled, while a new index has them enabled by default. The full rules and an example of merging an updated index are presented on the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.

## Merging example

The following example demonstrates the merging of two indexes.

**C#**

```csharp
string indexFolder1 = @"c:\MyIndex1\";
string indexFolder2 = @"c:\MyIndex2\";
string documentsFolder1 = @"c:\MyDocuments1\";
string documentsFolder2 = @"c:\MyDocuments2\";
 
Index index1 = new Index(indexFolder1); // Creating index1
index1.Add(documentsFolder1); // Indexing documents
 
Index index2 = new Index(indexFolder2); // Creating index2
index2.Add(documentsFolder2); // Indexing documents
 
MergeOptions options = new MergeOptions();
options.Cancellation = new Cancellation(); // Creating cancellation object to be able to cancel the operation
 
try
{
    // Merging index2 into index1
    // Files of index2 will not be changed
    index1.Merge(index2, options);
}
catch (InvalidOperationException exception)
{
    // The indexes divide text into terms differently; the message names the differences
    Console.WriteLine(exception.Message);
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

* [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

* [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
