---
id: migration-notes
url: search/java/migration-notes
title: Migration Notes
weight: 3
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
### Why To Migrate?

Here are the main reasons for using the new updated API provided by GroupDocs.Search for Java from version 19.10:

*   Unified work with some similar entities to increase the intuitiveness of using API.
*   The architecture of the product has been revised and optimized, so that some functions will work faster.
*   Some entities have been renamed to improve code readability.
*   The changes made to the API are not too significant, so the migration will not be too difficult.
*   New functionality will not be added to the old API version, only to the new version.

### How To Migrate?

The following code examples demonstrate changes in the use of the API.

Old coding style:



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.addToIndex(documentsFolder);
 
// Creating a search parameters object
SearchParameters parameters = new SearchParameters();
parameters.getFuzzySearch().setEnabled(true); // Enabling the fuzzy search
parameters.getFuzzySearch().setFuzzyAlgorithm(new TableDiscreteFunction(2)); // Setting the number of possible differences for each word
 
// Searching in the index
SearchResults results = index.search("Happiness", parameters);
 
if (results.getCount() > 0) {
    // Generating HTML-formatted text of a document with highlighted search results
    index.highlightInText("c:\\Highlighted.html", results.get_Item(0));
}
```

New coding style:



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Creating a search options object
SearchOptions options = new SearchOptions();
options.getFuzzySearch().setEnabled(true); // Enabling the fuzzy search
options.getFuzzySearch().setFuzzyAlgorithm(new TableDiscreteFunction(2)); // Setting the number of possible differences for each word
 
// Searching in the index
SearchResult result = index.search("Happiness", options);
 
if (result.getDocumentCount() > 0) {
    // Generating HTML-formatted text of a document with highlighted search results
    index.highlight(result.getFoundDocument(0), new HtmlHighlighter(new FileOutputAdapter("c:\\Highlighted.html")));
}
```
