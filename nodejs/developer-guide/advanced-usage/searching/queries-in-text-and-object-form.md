---
id: queries-in-text-and-object-form
url: search/nodejs-java/queries-in-text-and-object-form
title: Queries in text and object form
weight: 15
description: "This article gives the knowledge about two ways to create a search query: in text or object form using Java search API."
keywords: Queries in text and object form
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
There are two ways to create a search query: in text or object form.

A search query in text form is simpler but less flexible. The flexibility of queries in object form lies in the fact that it is possible for each subquery to set separate search options. In object form, you can also use, for example, regular expression query as a part of more complex one. All combinations of nesting search queries in object form can be found on the page [Nesting search queries in object form]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/nesting-search-queries-in-object-form.md" >}}).

The correlation of two forms of search queries is that each text query before search is transformed to query in object form. Therefore, processor handles queries in object form faster.

The full specification of the query language in text form is presented on the page [Query language specification]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/query-language-specification.md" >}}). Syntax with examples of all elements allowed in text search queries is presented on the page [Search operation table]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/search-operation-table.md" >}}).

Each example in this documentation is usually given with the search query in text and object form.

The example of complex query in object form is given below.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating index
const index = new groupdocs.search.Index(indexFolder);

// Indexing
index.add(documentsFolder);

// Creating subquery 1 - simple word query
const subquery1 = groupdocs.search.SearchQuery.createWordQuery('future');
subquery1.setSearchOptions(new groupdocs.search.SearchOptions()); // Setting search options only for subquery 1
subquery1.getSearchOptions().getFuzzySearch().setEnabled(true);
subquery1.getSearchOptions().getFuzzySearch().setFuzzyAlgorithm(new groupdocs.search.TableDiscreteFunction(3)); // The maximum number of differences is 3

// Creating subquery 2 - wildcard query
const subquery2 = groupdocs.search.SearchQuery.createWildcardQuery(1);

// Creating subquery 3 - regular expression query
const subquery3 = groupdocs.search.SearchQuery.createRegexQuery('(.)\\1');

// Combining subqueries into one query - phrase search query
const query = groupdocs.search.SearchQuery.createPhraseSearchQuery(subquery1, subquery2, subquery3);

// Creating overall search options with increased capacity of occurrences
const options = new groupdocs.search.SearchOptions();
options.setMaxOccurrenceCountPerTerm(1000000);
options.setMaxTotalOccurrenceCount(10000000);

// Searching
const result = index.search(query, options);

// The result may contain the following word sequences:
// futile * blessed
// father * excellent
// tyre * assyria
// return * 229
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
