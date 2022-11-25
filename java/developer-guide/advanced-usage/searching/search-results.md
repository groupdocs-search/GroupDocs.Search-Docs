---
id: search-results
url: search/java/search-results
title: Search results
weight: 24
description: "This article shows that how to perform the operations on search results."
keywords: Search results
productName: GroupDocs.Search for Java
hideChildren: False
---
Search results are represented by the [SearchResult](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult) class, an instance of which is returned by the [search](https://reference.groupdocs.com/search/java/com.groupdocs.search/Index#search(java.lang.String)) method of the [Index](https://reference.groupdocs.com/search/java/com.groupdocs.search/Index) class. The [search](https://reference.groupdocs.com/search/java/com.groupdocs.search/IndexRepository#search(java.lang.String)) method of the [IndexRepository](https://reference.groupdocs.com/search/java/com.groupdocs.search/IndexRepository) class also returns an instance of the [SearchResult](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult) class.

The [SearchResult](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult) class contains the following members:

*   The [getDocumentCount](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult#getDocumentCount()) method returns the number of documents found.
*   The [getOccurrenceCount](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult#getOccurrenceCount()) method returns the total number of occurrences found.
*   The [getTruncated](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult#getTruncated()) method returns a value indicating that the result is truncated due to limits specified in the search options.
*   The [getWarnings](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult#getWarnings()) method returns a warnings describing the result, for example, a warning about the presence of stop word in a search query.
*   The [getNextChunkSearchToken](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult#getNextChunkSearchToken()) method returns a chunk search token to search for the next chunk. For details on search by chunks, see the [Search by chunks]({{< ref "search/java/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) page.
*   The [getStartTime](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult#getStartTime()) method returns the start time of the search.
*   The [getEndTime](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult#getEndTime()) method returns the end time of the search.
*   The [getSearchDuration](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult#getSearchDuration()) method returns the search duration.
*   The [getFoundDocument](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/SearchResult#getFoundDocument(int)) method returns the found document by index.

The found document is represented by an instance of the [FoundDocument](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument) class. The [FoundDocument](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument) class contains the following members:

*   The [getDocumentInfo](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument#getDocumentInfo()) method returns the document info object containing the file path, the file type, the format family, and the inner document path for items of container documents.
*   The [getRelevance](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument#getRelevance()) method returns the relevance of the document in the search result.
*   The [getOccurrenceCount](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument#getOccurrenceCount()) method returns the number of occurrences found in the document.
*   The [getFoundFields](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument#getFoundFields()) method returns the found document fields.
*   The [getTerms](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument#getTerms()) method returns the found terms. The value is evaluated each time the property is accessed based on the data for each document field found.
*   The [getTermSequences](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument#getTermSequences()) method returns the found term sequences.
*   The [serialize](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument#serialize()) method serializes the current found document instance to a byte array.
*   The [deserialize](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocument#deserialize(byte[])) method deserializes an instance of found document from a byte array.

The found document field is represented by an instance of the [FoundDocumentField](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField) class. The [FoundDocumentField](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField) class contains the following members:

*   The [getFieldName](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField#getFieldName()) method returns the field name.
*   The [getOccurrenceCount](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField#getOccurrenceCount()) method returns the number of occurrences found.
*   The [getTerms](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField#getTerms()) method returns the terms found.
*   The [getTermsOccurrences](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField#getTermsOccurrences()) method returns the occurrences of the found terms.
*   The [getTermSequences](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField#getTermSequences()) method returns the term sequences found.
*   The [getTermSequencesOccurrences](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField#getTermSequencesOccurrences()) method returns the occurrences of the found term sequences.
*   The [serialize](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField#serialize()) method serializes the current found document field instance to a byte array.
*   The [deserialize](https://reference.groupdocs.com/search/java/com.groupdocs.search.results/FoundDocumentField#deserialize(byte[])) method deserializes an instance of found document field from a byte array.

The following example shows how to print information on the documents found in the console.



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
    for (FoundDocumentField field : document.getFoundFields()) {
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
                for (String term : terms) {
                    sequence += term + " ";
                }
                System.out.println("\t\t\t" + sequence + " - " + field.getTermSequencesOccurrences()[k]);
            }
        }
    }
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
