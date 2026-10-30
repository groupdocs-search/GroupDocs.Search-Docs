---
id: search-flow
url: search/python-net/search-flow
title: Search flow
weight: 19
description: "This article describes the internal stages of each search operation."
keywords: Search flow
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
The table below shows the internal stages of each search operation.

Before a search query in text form is parsed, aliases in the query are replaced with their text (the ALIAS_SUBSTITUTION phase of the SearchPhase enumeration). This phase is performed for every search query in text form and is not performed for queries in object form. For details on aliases, see the [Using aliases]({{< ref "search/python-net/developer-guide/advanced-usage/searching/using-aliases.md" >}}) page.

The completion of each phase raises the [search_phase_completed](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/search_phase_completed/) event. The [search_phase](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/searchphaseeventargs/search_phase/) property of the event arguments returns the completed phase, the [query](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/searchphaseeventargs/query/) property returns the processed query or word, and the [words](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/searchphaseeventargs/words/) property returns the words obtained in this phase. For details, see the [Search index events]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-events.md#search_phase_completed-event" >}}) page.

See also [Query language specification]({{< ref "search/python-net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}), [Search operation table]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-operation-table.md" >}}).

| Operation | Search flow |
| --- | --- |
| Any search query in text form | Alias substitution<br/>Parsing the query<br/>Search flow of the parsed query (see below) |
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

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
