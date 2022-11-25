---
id: work-with-search-results
url: search/java/work-with-search-results
title: Work with search results
weight: 3
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
Working with search results consists in obtaining information from objects of search results and highlighting occurrences in the text of documents.

## Obtain search result information

When a search is complete, the [search](https://reference.groupdocs.com/search/java/com.groupdocs.search/Index#search(java.lang.String)) method returns an object of type [SearchResult](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult). This page describes the information available in an object of type [SearchResult](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult).

From the root object of the search result, information is available on the number of documents found, the number of occurrences of the words and phrases found, as well as detailed information on each individual document. Information about an individual document found is represented by an object of the class [FoundDocument](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument).

From the object of the class [FoundDocument](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument), information is available on the full path to the document, the number of occurrences, the words and phrases found, as well as detailed information on each field of the document. Information about the document field is represented by an object of the class [FoundDocumentField](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField).

From the object of the class [FoundDocumentField](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField), information is available on the name of the document field, the number of occurrences, the words and phrases found, and the number of occurrences of each word and phrase.

Below is a code example that writes to the console the detailed information contained in an object of the class [SearchResult](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult) about each document, document field, word, phrase and the number of their occurrences in the document fields.



```java
String indexFolder = "c:\\MyIndex\\";
String documentFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentFolder);
 
// Creating search options
SearchOptions options = new SearchOptions();
options.getFuzzySearch().setEnabled(true); // Enabling the fuzzy search
options.getFuzzySearch().setFuzzyAlgorithm(new TableDiscreteFunction(3)); // Setting the maximum number of differences to 3
 
// Search for documents containing the word 'Einstein' or the phrase 'Theory of Relativity'
SearchResult result = index.search("Einstein OR \"Theory of Relativity\"", options);
 
// Printing the result
System.out.println("Documents: " + result.getDocumentCount());
System.out.println("Total occurrences: " + result.getOccurrenceCount());
for (int i = 0; i < result.getDocumentCount(); i++) {
    FoundDocument document = result.getFoundDocument(i);
    System.out.println("\tDocument: " + document.getDocumentInfo().getFilePath());
    System.out.println("\tOccurrences: " + document.getOccurrenceCount());
    for (int j = 0; j < document.getFoundFields().length; j++) {
        FoundDocumentField field = document.getFoundFields()[j];
        System.out.println("\t\tField: " + field.getFieldName());
        System.out.println("\t\tOccurrences: " + document.getOccurrenceCount());
        // Printing found terms
        if (field.getTerms() != null) {
            for (int k = 0; k < field.getTerms().length; k++) {
                System.out.println("\t\t\t" + field.getTerms()[k] + " - " + field.getTermsOccurrences()[k]);
            }
        }
        // Printing found phrases
        if (field.getTermSequences() != null) {
            for (int k = 0; k < field.getTermSequences().length; k++) {
                String[] terms = field.getTermSequences()[k];
                String sequence = "";
                for (int m = 0; m < terms.length; m++) {
                    sequence += terms[m] + " ";
                }
                System.out.println("\t\t\t" + sequence + " - " + field.getTermSequencesOccurrences()[k]);
            }
        }
    }
}
```

## Highlight search results

At the end of a search, it is usually necessary to visualize the results by highlighting the words found in the text. You can highlight search results either in the text of an entire document, or in separate segments. Detailed information about highlighting search results can be found on the page [Highlighting search results]({{< ref "search/java/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}). Below is an example of highlighting search results in the text of an entire document.



```java
String indexFolder = "c:\\MyIndex\\";
String documentFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentFolder);
 
// Search for the word 'eternity'
SearchResult result = index.search("eternity");
 
// Highlighting occurrences in text
if (result.getDocumentCount() > 0) {
    FoundDocument document = result.getFoundDocument(0); // Getting the first found document
    OutputAdapter outputAdapter = new FileOutputAdapter("c:\\Highlighted.html"); // Creating an output adapter to the file
    Highlighter highlighter = new HtmlHighlighter(outputAdapter); // Creating the highlighter object
    index.highlight(document, highlighter); // Generating HTML formatted text with highlighted occurrences
}
```

## More resources

### Advanced usage topics

To learn more about search features and get familiar how to enhance your search solution, please refer to the [advanced usage section]({{< ref "search/java/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
