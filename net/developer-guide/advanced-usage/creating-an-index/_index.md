---
id: creating-an-index
url: search/net/creating-an-index
title: Creating an index
weight: 1
description: ""
keywords: 
productName: GroupDocs.Search for .NET
hideChildren: False
---
The articles in this section describe advanced search index creation topics.

Find out how to create an index on disk or in memory, subscribe to index events, work with multiple indexes in a single index repository, apply index settings, and which indexes of previous versions can be opened, updated, merged and saved:

*   [Search index location]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-location.md" >}})
*   [Search index events]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}})
*   [Search index repository]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-repository.md" >}})
*   [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}})
*   [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}})

{{< alert style="info" >}}Since version 26.11, an index folder is used by one [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) object at a time, in this process or in another one: dispose of an index before its folder is opened, created, saved or updated again; otherwise, [IOException](https://learn.microsoft.com/dotnet/api/system.io.ioexception) is thrown. An index that is not disposed keeps its folder until it is finalized; an index loaded into memory by [LoadIntoMemoryCompletely](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) does not use its folder. A new index is not created in a folder that contains other files or subfolders unless *overwriteIfExists* is *true*. See [Search index location]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-location.md#index-folder" >}}).{{< /alert >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
