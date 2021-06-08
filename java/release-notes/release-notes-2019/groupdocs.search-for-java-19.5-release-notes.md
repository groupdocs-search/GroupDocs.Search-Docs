---
id: groupdocs-search-for-java-19-5-release-notes
url: search/java/groupdocs-search-for-java-19-5-release-notes
title: GroupDocs.Search for Java 19.5 Release Notes
weight: 3
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 19.5{{< /alert >}}

## Major Features

There are the following improvements and new features in this release:

*   Implement document filtering in search results
*   Implement settings for logging functionality
*   Add English synonyms to default synonym dictionary
*   Implement providing number of hits for each found word separately
*   Implement optimization of index storage format
*   Add support for new file formats
*   Implement method to retrieve credit consumption info
*   Implement ability to attach to a document arbitrary additional fields

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-371 | Implement document filtering in search results | Improvement |
| SEARCHNET-1883 | Implement settings for logging functionality | Improvement |
| SEARCHNET-1896 | Add English synonyms to default synonym dictionary | Improvement |
| SEARCHNET-1945 | Implement providing number of hits for each found word separately | Improvement |
| SEARCHNET-1951 | Implement optimization of index storage format | Improvement |
| SEARCHNET-1966 | Add support for new file formats | Improvement |
| SEARCHNET-1980 | Implement method to retrieve credit consumption info | Improvement |
| SEARCHNET-1941 | Implement ability to attach to a document arbitrary additional fields | New Feature |

  

Public API and Backward Incompatible Changes

{{< alert style="info" >}}This section lists public API changes that were introduced in GroupDocs.Search for Java 19.5. It includes not only new and obsoleted public methods, but also a description of any changes in the behavior behind the scenes in GroupDocs.Search which may affect existing code. Any behavior introduced that could be seen as a regression and modifies existing behavior is especially important and is documented here.{{< /alert >}}

### Implement document filtering in search results

This improvement allows to filter found documents.

##### Public API changes

Inverface **ISearchDocumentFilter** has been added to **com.groupdocs.search** package.

Class **SearchDocumentFilter** has been added to **com.groupdocs.search** package.  
Static method ****ISearchDocumentFilter** createFileNameRegularExpression(String)** has been added to ********com.groupdocs.search.****SearchDocumentFilter******** class.  
Static method ****ISearchDocumentFilter** createFileNameRegularExpression(String, RegexOptions)** has been added to ********com.groupdocs.search.****SearchDocumentFilter******** class.  
Static method ****ISearchDocumentFilter** createFileExtension(String\[\])** has been added to ********com.groupdocs.search.****SearchDocumentFilter******** class.  
Static method ****ISearchDocumentFilter** createInverted(****ISearchDocumentFilter****)** has been added to ********com.groupdocs.search.****SearchDocumentFilter******** class.  
Static method ****ISearchDocumentFilter** createConjunction(****ISearchDocumentFilter\[\]****)** has been added to ********com.groupdocs.search.****SearchDocumentFilter******** class.  
Static method ****ISearchDocumentFilter** createDisjunction(******ISearchDocumentFilter\[\]******)** has been added to ********com.groupdocs.search.****SearchDocumentFilter******** class.

Method **ISearchDocumentFilter**** get****SearchDocumentFilter()****** has been added to **com.groupdocs.search.SearchParameters** class.  
Method **void**** set****SearchDocumentFilter(ISearchDocumentFilter value)****** has been added to **com.groupdocs.search.SearchParameters** class.

##### Usecases

This example shows how to set up document filtering for searching:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating index
Index index = new Index(indexFolder);

// Adding documents to index
index.addToIndex(documentFolder);

// Configuring document filter
// Filter filter3 will only accept TXT and DOC documents with text 'Tolkien' in file names
SearchParameters parameters = new SearchParameters();
ISearchDocumentFilter filter1 = SearchDocumentFilter.createFileExtension(".txt", ".doc");
ISearchDocumentFilter filter2 = SearchDocumentFilter.createFileNameRegularExpression("Tolkien");
ISearchDocumentFilter filter3 = SearchDocumentFilter.createConjunction(filter1, filter2);
parameters.setSearchDocumentFilter(filter3);

// Searching
SearchResults results = index.search("big", parameters);
```

### Implement settings for logging functionality

This improvement allows to set up log file name and maximum log file size.

##### Public API changes

Class **LogSettings** has been added to **com.groupdocs.search** package.  
Method **String getFileName()** has been added to ******com.groupdocs.search****.LogSettings****** class.  
Method **void setFileName(String value)** has been added to ******com.groupdocs.search****.LogSettings****** class.  
Method **double getMaxSize()** has been added to ******com.groupdocs.search****.LogSettings****** class.  
Method **void setMaxSize(double value)** has been added to ******com.groupdocs.search****.LogSettings****** class.

Method ********LogSettings****** get******LogSettings()******** has been added to ****com.groupdocs.search**.Index** class.

##### Usecases

This example shows how to configure logging settings:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating index
Index index = new Index(indexFolder);

// Setting the log file name. The log file name can be relative or absolute.
index.getLogSettings().setFileName("Log\\Log.txt");

// Setting the maximum size of the log file in megabytes. This value must be in the range from 0.1 to 1000.
index.getLogSettings().setMaxSize(2.0);

// Adding documents to index
index.addToIndex(documentFolder);

// Searching
SearchResults results = index.search("big");
```

### Add English synonyms to default synonym dictionary

This improvement adds English synonyms to default synonym dictionary.

##### Public API changes

None.

##### Usecases

This example shows how to perform synonym search:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating index
// Default synonyms will be added automatically to the index
Index index = new Index(indexFolder);

// Adding documents to index
index.addToIndex(documentFolder);

// Enabling synonym search in search parameters
SearchParameters parameters = new SearchParameters();
parameters.setUseSynonymSearch(true);

// Searching for word 'big'
// The word 'large' will also be found
SearchResults results = index.search("big", parameters);
```

### Implement providing number of hits for each found word separately

This improvement adds exact number of occurrences for each found word. This can be used to implement 'Did you mean' feature.

##### Public API changes

Method **int\[\] getTermsOccurrences()** has been added to **com.groupdocs.search.DetailedResultInfo** class.  
Method **int\[\] getTermSequencesOccurrences()** has been added to **com.groupdocs.search.DetailedResultInfo** class.

##### Usecases

This example shows how to get number of occurrences for each found word:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating index
Index index = new Index(indexFolder);

// Adding documents to index
index.addToIndex(documentFolder);

// Configuring search parameters
SearchParameters parameters = new SearchParameters();
// Enabling maximum 2 misprints per word
parameters.getFuzzySearch().setFuzzyAlgorithm(new TableDiscreteFunction(2));
parameters.getFuzzySearch().setEnabled(true);

// Searching
SearchResults results = index.search("hobbot", parameters);

// Getting number of occurrences for each found word
for (DocumentResultInfo doc : results) {
    for (DetailedResultInfo field : doc.getDetailedResults()) {
        for (int i = 0; i < field.getTerms().length; i++) {
            String word = field.getTerms()[i];
            int hits = field.getTermsOccurrences()[i];
            System.out.println(word + " - " + hits);
        }
    }
}
```

### Implement optimization of index storage format

This improvement reduces index storage size more than by 30%.

##### Public API changes

Class **VersionUpdateResult** has been added to **com.groupdocs.search** package.  
Static field **int Updated** has been added to ****com.groupdocs.search**.**VersionUpdateResult**** class.  
Static field **int ****AlreadyUpToDate** has been added to ****com.groupdocs.search**.**VersionUpdateResult**** class.  
Static field **int ****Unsupported** has been added to ****com.groupdocs.search**.**VersionUpdateResult**** class.

Class **IndexVersionUpdater** has been added to **com.groupdocs.search** package.  
Constructor **IndexVersionUpdater()** has been added to ******com.groupdocs.search****.**IndexVersionUpdater**** class.  
Method **bool isLatestVersion(String)** has been added to ******com.groupdocs.search****.**IndexVersionUpdater**** class.  
Method **bool canUpdate(String)** has been added to ******com.groupdocs.search****.**IndexVersionUpdater**** class.  
Method **int update(String, String)** has been added to **GroupDocs.Search.**IndexVersionUpdater**** class.

##### Usecases

This example shows how to update index of previous version:



```java
String oldIndexFolder = "c:\\MyIndex";
String newIndexFolder = "c:\\MyIndexUpdated";

// Creating updater instance
IndexVersionUpdater updater = new IndexVersionUpdater();

// Updating index version
if (updater.canUpdate(oldIndexFolder)) {
    int updateResult = updater.update(oldIndexFolder, newIndexFolder);
    System.out.println("Updated: " + (updateResult == VersionUpdateResult.Updated));
}

// Loading updated index
Index index = new Index(newIndexFolder);

// Searching
SearchResults results = index.search("hobbit");
```

### Add support for new file formats

This improvement adds support for new file formats: MD, POTM, CSV, TSV, XML, HTM, HTML, XHTML, MHT, MHTML.

##### Public API changes

Field **String Xml** has been added to **com.groupdocs.search.DocumentType** class.

Field **String Xml** has been added to **com.groupdocs.search.DocumentTypes** class.

##### Usecases

This example shows how to add documents to an index:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating index
Index index = new Index(indexFolder);

// Adding documents to index
index.addToIndex(documentFolder);

// Searching
SearchResults results = index.search("hobbit");
```

### Implement method to retrieve credit consumption info

This improvement implements method to retrieve credit consumption info.

##### Public API changes

Method **double getConsumptionCredit()** has been added to **com.groupdocs.search.Metered** class.

##### Usecases

This example shows how to check how many credits are spent:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Set Metered keys
new Metered().setMeteredKey(publicKey, privateKey);
 
// Creating index
Index index = new Index(indexFolder);

// Adding documents to index
index.addToIndex(documentFolder);

// Searching
SearchResults results = index.search("arbitrary");

// Getting consumption credit
double used = Metered.getConsumptionCredit();
```

### Implement ability to attach to a document arbitrary additional fields

This feature allows adding arbitrary additional fields to each document during indexing. Field names can be non-unique.

##### Public API changes

Method **FieldInfo\[\] getAdditionalFields()** has been added to **com.groupdocs.search.FileIndexingEventArgs** class.  
Method **void setAdditionalFields(FieldInfo\[\])** has been added to **com.groupdocs.search.FileIndexingEventArgs** class.  
Method **String highlightInText(DocumentResultInfo, IFieldExtractor, FieldInfo\[\])** has been added to ******com.groupdocs.search****.Index****** class.  
Method **void highlightInText(String, DocumentResultInfo, IFieldExtractor, FieldInfo\[\])** has been added to ******com.groupdocs.search****.Index****** class.  
Method **String extractDocumentText(DocumentInfo, IFieldExtractor, FieldInfo\[\])** has been added to ******com.groupdocs.search****.Index****** class.  
Method **void extractDocumentText(String, **DocumentInfo**, IFieldExtractor, FieldInfo\[\])** has been added to ******com.groupdocs.search****.Index****** class.

##### Usecases

This example shows how to add additional fields to a document during indexing:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";

// Creating index
Index index = new Index(indexFolder);

// Subscribing to event
index.FileIndexing.add(new EventHandler<FileIndexingEventArgs>() {
    @Override
    public void invoke(Object sender, FileIndexingEventArgs args) {
        com.groupdocs.search.FieldInfo[] additionalFields = new com.groupdocs.search.FieldInfo[] {
            new com.groupdocs.search.FieldInfo("Tags", "arbitrary additional fields"),
        };
        args.setAdditionalFields(additionalFields);
    }
});

// Adding documents to index
index.addToIndex(documentFolder);

// Searching
SearchResults results = index.search("arbitrary");
```
