---
id: work-with-search-results
url: search/nodejs-java/work-with-search-results
title: Work with search results
weight: 3
description: ""
keywords: 
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
Working with search results consists in obtaining information from objects of search results and highlighting occurrences in the text of documents.

## Obtain search result information

When a search is complete, the [search](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search/Index#search(java.lang.String)) method returns an object of type [SearchResult](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.results/SearchResult). This page describes the information available in an object of type [SearchResult](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.results/SearchResult).

From the root object of the search result, information is available on the number of documents found, the number of occurrences of the words and phrases found, as well as detailed information on each individual document. Information about an individual document found is represented by an object of the class [FoundDocument](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.results/FoundDocument).

From the object of the class [FoundDocument](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.results/FoundDocument), information is available on the full path to the document, the number of occurrences, the words and phrases found, as well as detailed information on each field of the document. Information about the document field is represented by an object of the class [FoundDocumentField](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.results/FoundDocumentField).

From the object of the class [FoundDocumentField](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.results/FoundDocumentField), information is available on the name of the document field, the number of occurrences, the words and phrases found, and the number of occurrences of each word and phrase.

Below is a code example that writes to the console the detailed information contained in an object of the class [SearchResult](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.results/SearchResult) about each document, document field, word, phrase and the number of their occurrences in the document fields.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentFolder = 'c:/MyDocuments/';
 
// Creating an index
const index = new groupdocs.search.Index(indexFolder);

// Indexing documents from the specified folder
index.add(documentFolder);

// Creating search options
const options = new groupdocs.search.SearchOptions();
options.getFuzzySearch().setEnabled(true); // Enabling the fuzzy search
options.getFuzzySearch().setFuzzyAlgorithm(new groupdocs.search.TableDiscreteFunction(3)); // Setting the maximum number of differences to 3

// Search for documents containing the word 'favourable' or the phrase 'ipsum dolor'
const result = index.search('favourable OR "ipsum dolor"', options);

// Printing the result
console.log();
console.log('Documents: ' + result.getDocumentCount());
console.log('Total occurrences: ' + result.getOccurrenceCount());
for (let i = 0; i < result.getDocumentCount(); i++) {
  const document = result.getFoundDocument(i);
  console.log('\tDocument: ' + document.getDocumentInfo().getFilePath());
  console.log('\tOccurrences: ' + document.getOccurrenceCount());
  const fields = document.getFoundFields();
  for (let j = 0; j < fields.length; j++) {
    const field = fields[j];
    console.log('\t\tField: ' + field.getFieldName());
    console.log('\t\tOccurrences: ' + document.getOccurrenceCount());
    // Printing found terms
    if (field.getTerms() != null) {
      for (let k = 0; k < field.getTerms().length; k++) {
        console.log('\t\t\t' + field.getTerms()[k] + ' - ' + field.getTermsOccurrences()[k]);
      }
    }
    // Printing found phrases
    if (field.getTermSequences() != null) {
      for (let k = 0; k < field.getTermSequences().length; k++) {
        const terms = field.getTermSequences()[k];
        let sequence = '';
        for (let m = 0; m < terms.length; m++) {
          const term = terms[m];
          sequence += term + ' ';
        }
        console.log('\t\t\t' + sequence + ' - ' + field.getTermSequencesOccurrences()[k]);
      }
    }
  }
}
```

## Highlight search results

At the end of a search, it is usually necessary to visualize the results by highlighting the words found in the text. You can highlight search results either in the text of an entire document, or in separate segments. Detailed information about highlighting search results can be found on the page [Highlighting search results]({{< ref "search/nodejs-java/developer-guide/advanced-usage/searching/highlighting-search-results.md" >}}). Below is an example of highlighting search results in the text of an entire document.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentFolder = 'c:/MyDocuments/';

// Creating an index settings instance
const settings = new groupdocs.search.IndexSettings();
settings.setTextStorageSettings(new groupdocs.search.TextStorageSettings(groupdocs.search.Compression.High)); // Enabling the storage of extracted text in the index

// Creating an index in the specified folder
const index = new groupdocs.search.Index(indexFolder, settings);

// Indexing documents from the specified folder
index.add(documentFolder);

// Search for the word 'eternity'
const result = index.search('eternity');

// Highlighting occurrences in text
if (result.getDocumentCount() > 0) {
  const document = result.getFoundDocument(0); // Getting the first found document
  const localPath = Utils.OutputPath + 'BasicUsage/WorkWithSearchResults/Highlighted.html';
  const outputAdapter = new groupdocs.search.FileOutputAdapter(groupdocs.search.OutputFormat.Html, localPath); // Creating an output adapter to the file
  const highlighter = new groupdocs.search.DocumentHighlighter(outputAdapter); // Creating the highlighter object
  index.highlight(document, highlighter); // Generating HTML formatted text with highlighted occurrences
}
```

## More resources

### Advanced usage topics

To learn more about search features and get familiar how to enhance your search solution, please refer to the [advanced usage section]({{< ref "search/nodejs-java/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
