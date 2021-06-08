---
id: groupdocs-search-for-java-20-4-release-notes
url: search/java/groupdocs-search-for-java-20-4-release-notes
title: GroupDocs.Search for Java 20.4 Release Notes
weight: 1
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for Java 20.4{{< /alert >}}

## Major Features

There are the following new features and improvements in this release:

*   Add MailFieldNames class containing e-mail field names
*   Implement index statuses reflecting possible operations
*   Remove Legacy namespace
*   Implement options for metadata indexing
*   Implement changing path without reindexing for renamed documents
*   Implement ability to change attributes of indexed documents without reindexing

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2127 | Add MailFieldNames class containing e-mail field names | Improvement |
| SEARCHNET-2160 | Implement index statuses reflecting possible operations | Improvement |
| SEARCHNET-2185 | Remove Legacy namespace | Improvement |
| SEARCHNET-2186 | Implement options for metadata indexing | Improvement |
| SEARCHNET-215 | Implement changing path without reindexing for renamed documents | New Feature |
| SEARCHNET-2134 | Implement ability to change attributes of indexed documents without reindexing | New Feature |

## Public API and Backward Incompatible Changes

{{< alert style="info" >}}This section lists public API changes that were introduced in GroupDocs.Search for Java 20.4. It includes not only new and obsoleted public methods, but also a description of any changes in the behavior behind the scenes in GroupDocs.Search which may affect existing code. Any behavior introduced that could be seen as a regression and modifies existing behavior is especially important and is documented here.{{< /alert >}}

### Add MailFieldNames class containing e-mail field names

This improvement adds the **MailFieldNames** class, which contains constants with values of field names of mail documents.

##### Public API changes

Static class **MailFieldNames** has been added to **com.groupdocs.search.options** package.  
Constant **String MailMessageBody** has been added to **com.groupdocs.search.options.MailFieldNames** class.  
Constant **String MailSenderName** has been added to **com.groupdocs.search.options.MailFieldNames** class.  
Constant **String MailDisplayTo** has been added to **com.groupdocs.search.options.MailFieldNames** class.  
Constant **String MailSubject** has been added to **com.groupdocs.search.options.MailFieldNames** class.

##### Use cases

This example shows how to use field names constants to search specific fields of documents:



```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating index
Index index = new Index(indexFolder);
 
// Adding documents to index
index.add(documentFolder);
 
// Searching
SearchResult result = index.search(MailFieldNames.MailMessageBody + ": meeting");
```

### Implement index statuses reflecting possible operations

This improvement brings order to the set of statuses of the index, and also clarifies the type of operation being performed.

##### Public API changes

Field **NotStarted** has been deleted from **com.groupdocs.search.common.IndexStatus** class.  
Field **InProgress** has been deleted from **com.groupdocs.search.common.IndexStatus** class.  
Field **LicenseRestrictionFinished** has been deleted from **com.groupdocs.search.common.IndexStatus** class.  
Field **Indexing** has been added to **com.groupdocs.search.common.IndexStatus** class.  
Field **Updating** has been added to **com.groupdocs.search.common.IndexStatus** class.  
Field **Merging** has been added to **com.groupdocs.search.common.IndexStatus** class.  
Field **Optimizing** has been added to **com.groupdocs.search.common.IndexStatus** class.  
Field **Deleting** has been added to **com.groupdocs.search.common.IndexStatus** class.

Field **Deleting** has been added to **com.groupdocs.search.events.OperationType** class.  
Field **Optimizing** has been added to **com.groupdocs.search.events.OperationType** class.

##### Use cases

This example shows how to get information about changing the status of an index and completing the index operation:

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating index
Index index = new Index(indexFolder);
 
// Subscribing to event
index.getEvents().StatusChanged.add(new EventHandler<BaseIndexEventArgs>() {
    public void invoke(Object sender, BaseIndexEventArgs args) {
        System.out.println("Index status: " + args.getStatus());
    }
});
 
// Subscribing to event
index.getEvents().OperationFinished.add(new EventHandler<OperationFinishedEventArgs>() {
    public void invoke(Object sender, OperationFinishedEventArgs args) {
        System.out.println("Operation finished: " + args.getOperationType());
    }
});
 
// Starting indexing operation
index.add(documentFolder);
```

### Remove Legacy namespace

All types from the **com.groupdocs.search.legacy** package have been removed.

##### Public API changes

All types from the **com.groupdocs.search.legacy** package have been removed.

##### Use cases

None.

### Implement options for metadata indexing

This improvement adds the **getMetadataIndexingOptions()** method to **IndexingOptions**, **UpdateOptions**, and **TextOptions** classes. The **MetadataIndexingOptions** class contains the following methods for setting metadata indexing options:

*   The **setIndexingEmptyValues(boolean)** property sets a value indicating whether to index empty field values or not. The default value is true.
*   The **setIndexingEmptyNames(boolean)** property sets a value indicating whether to index empty field names or not. The default value is true.
*   The **setDefaultFieldName(String)** property sets the default field name used to index empty field names. The default value is "unknown".
*   The **setSeparatorInCompoundName(String)** property sets the separator in the compound name of a field. The default value is "." (period character).
*   The **setMaxBytesToIndexField(int)** property sets the maximum number of values indexed from an array of **byte** values. The default value is **int.MaxValue**.
*   The **setMaxIntsToIndexField(int)** property sets the maximum number of values indexed from an array of **int** values. The default value is **int.MaxValue**.
*   The **setMaxLongsToIndexField(int)** property sets the maximum number of values indexed from an array of **long** values. The default value is **int.MaxValue**.
*   The **setMaxDoublesToIndexField(int)** property sets the maximum number of values indexed from an array of **double** values. The default value is **int.MaxValue**.
*   The **setSeparatorBetweenValues(String)** property sets the separator between values in a field of type array. The default value is " " (space character).

##### Public API changes

Class **MetadataIndexingOptions** has been added to **com.groupdocs.search.options** package.  
Method **boolean getIndexingEmptyValues()** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **void setIndexingEmptyValues(boolean)** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **boolean getIndexingEmptyNames()** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **void setIndexingEmptyNames(boolean)** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **String getDefaultFieldName()** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **void setDefaultFieldName(String)** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **String getSeparatorInCompoundName()** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **void setSeparatorInCompoundName(String)** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **int getMaxBytesToIndexField()** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **void setMaxBytesToIndexField(int)** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **int getMaxIntsToIndexField()** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **void setMaxIntsToIndexField(int)** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **int getMaxLongsToIndexField()** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **void setMaxLongsToIndexField(int)** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **int getMaxDoublesToIndexField()** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **void setMaxDoublesToIndexField(int)** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **String getSeparatorBetweenValues()** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  
Method **void setSeparatorBetweenValues(String)** has been added to **com.groupdocs.search.options.MetadataIndexingOptions** class.  

Method **MetadataIndexingOptions getMetadataIndexingOptions()** has been added to **com.groupdocs.search.options.IndexingOptions** class.  
Method **MetadataIndexingOptions getMetadataIndexingOptions()** has been added to **com.groupdocs.search.options.TextOptions** class.  
Method **MetadataIndexingOptions getMetadataIndexingOptions()** has been added to **com.groupdocs.search.options.UpdateOptions** class.

##### Use cases

This example demonstrates how to set the metadata indexing options:

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Setting the metadata indexing options
IndexingOptions options = new IndexingOptions();
options.getMetadataIndexingOptions().setDefaultFieldName("default");
options.getMetadataIndexingOptions().setSeparatorInCompoundName("\\");
options.getMetadataIndexingOptions().setMaxBytesToIndexField(10);
options.getMetadataIndexingOptions().setMaxIntsToIndexField(10);
options.getMetadataIndexingOptions().setMaxLongsToIndexField(10);
options.getMetadataIndexingOptions().setMaxDoublesToIndexField(10);
 
// Starting indexing operation
index.add(documentFolder, options);
```

### Implement changing path without reindexing for renamed documents

This feature allows you to notify an index about renaming or moving to another location an indexed document. After notification, when calling the Update method, the document will not be reindexed even if its contents have changed.

##### Public API changes

Field **Renaming** has been added to **com.groupdocs.search.common.IndexStatus** class.

Class **Notification** has been added to **com.groupdocs.search.common** package.  
Method **Notification createRenameNotification(String, String)** has been added to **com.groupdocs.search.common.Notification** class.

Method **boolean notifyIndex(Notification)** has been added to **com.groupdocs.search.Index** class.

##### Use cases

This example demonstrates how to notify an index about renaming an indexed document:

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Indexing documents in a document folder
index.add(documentFolder);
 
// Renaming a document
String oldDocumentPath = "c:\\MyDocuments\\OldDocumentName.txt";
String newDocumentPath = "c:\\MyDocuments\\NewDocumentName.txt";
new File(oldDocumentPath).renameTo(new File(newDocumentPath));
 
// Notifying the index about renaming
Notification notification = Notification.createRenameNotification(oldDocumentPath, newDocumentPath);
boolean result = index.notifyIndex(notification);
 
// Updating the index
// The renamed document will not be reindexed
index.update();
```

### Implement ability to change attributes of indexed documents without reindexing

This feature allows you to add text attributes to indexed documents without the need to reindex documents. Added text attributes can be further used to filter search results.

##### Public API changes

Field **ChangingAttributes** has been added to **com.groupdocs.search.common.IndexStatus** class.

Class **AttributeChangeBatch** has been added to **com.groupdocs.search.common** package.  
Constructor **AttributeChangeBatch()** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **void add(String path, String\[\] ... attributes)** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **void add(String\[\] paths, String\[\] ... attributes)** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **void addToAll(String\[\] ... attributes)** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **void remove(String path, String\[\] ... attributes)** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **void remove(String\[\] paths, String\[\] ... attributes)** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **void removeAll(String path)** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **void removeAll(String\[\] paths)** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **void removeFromAll(String\[\] ... attributes)** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.  
Method **void clear()** has been added to **com.groupdocs.search.common.AttributeChangeBatch** class.

Method **void changeAttributes(AttributeChangeBatch batch)** has been added to **com.groupdocs.search.Index** class.  
Method **String\[\] getAttributes(String path)** has been added to **com.groupdocs.search.Index** class.

Method **String\[\] getAttributes()** has been added to **com.groupdocs.search.events.FileIndexingEventArgs** class.  
Method **void setAttributes(**String\[\]**)** has been added to **com.groupdocs.search.events.FileIndexingEventArgs** class.

Method **ISearchDocumentFilter createAttribute(String\[\] ... attributes)** has been added to **com.groupdocs.search.options.SearchDocumentFilter** class.

##### Use cases

This example demonstrates how to add and remove attributes from indexed documents:

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Indexing documents in a document folder
index.add(documentFolder);
 
// Creating an attribute change container object
AttributeChangeBatch batch = new AttributeChangeBatch();
// Adding one attribute to all indexed documents
batch.addToAll("public");
// Removing one attribute from one indexed document
batch.remove("c:\\MyDocuments\\KeyPoints.doc", "public");
// Adding two attributes to one indexed document
batch.add("c:\\MyDocuments\\KeyPoints.doc", "main", "key");
 
// Applying attribute changes in the index
index.changeAttributes(batch);
```

The next example demonstrates how to add attributes to documents during indexing and how to search with filter by attribute:

```java
String indexFolder = "c:\\MyIndex";
String documentFolder = "c:\\MyDocuments";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the FileIndexing event for adding attributes
index.getEvents().FileIndexing.add(new EventHandler<FileIndexingEventArgs>() {
    public void invoke(Object sender, FileIndexingEventArgs args) {
        if (args.getDocumentFullPath().endsWith("KeyPoints.doc")) {
            // Adding two attributes
            args.setAttributes(new String[] { "main", "key" });
        }
    }
});
 
// Indexing documents in a document folder
index.add(documentFolder);
 
// Searching in the index
SearchOptions options = new SearchOptions();
// Creating a document filter by attribute
options.setSearchDocumentFilter(SearchDocumentFilter.createAttribute("main"));
SearchResult result = index.search("Einstein", options);
```
