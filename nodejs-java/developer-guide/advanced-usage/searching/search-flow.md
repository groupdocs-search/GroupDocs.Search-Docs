---
id: search-flow
url: search/nodejs-java/search-flow
title: Search flow
weight: 19
description: "This article shows the internal stages of each search operation using Java search API."
keywords: Search flow
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
The table below shows the internal stages of each search operation. See also [Query language specification]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/query-language-specification.md" >}}), [Search operation table]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/search-operation-table.md" >}}).

| Operation | Search flow |
| --- | --- |
| Simple term search (case insensitive) | Keyboard layout correction<br/>Spelling correction<br/>Homophone search<br/>Synonym search<br/>Word forms search (since v.18.7)<br/>Fuzzy search<br/>Retrieving results |
| Simple term search (case sensitive) | Retrieving results |
| Wildcard search (since v.18.12) | Wildcard search<br/>Retrieving results |
| Date range search | Retrieving results |
| Numeric range search | Retrieving results |
| Phrase search | Retrieving results for each term of a phrase<br/>Joining sets of results |
| Regex search | Regex search<br/>Fuzzy search<br/>Retrieving results |
| And, Or | Retrieving results for each operand<br/>Combining sets of results |
| Not | Retrieving results for operand<br/>Inverting set of results |

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
