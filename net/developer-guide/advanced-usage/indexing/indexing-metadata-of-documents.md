---
id: indexing-metadata-of-documents
url: search/net/indexing-metadata-of-documents
title: Indexing metadata of documents
weight: 14
description: "This article explains how to create an index that contains only the metadata of documents by setting IndexType.MetadataIndex when creating the index."
keywords: metadata index, index only metadata
productName: GroupDocs.Search for .NET
hideChildren: False
---
GroupDocs.Search allows creating a full-text index or a metadata index of documents. To index only metadata without the main content of documents, set the [IndexType](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/indextype/) property of the index settings to [IndexType.MetadataIndex](https://reference.groupdocs.com/search/net/groupdocs.search.options/indextype/) when creating an index. The size of an index of this type will be very small. The index type is fixed when the index is created, so it cannot be changed for an existing index.

The following example demonstrates the creation of the metadata index.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an instance of index settings
IndexSettings settings = new IndexSettings();
settings.IndexType = IndexType.MetadataIndex; // Setting index type
 
// Creating a new index in the specified folder
Index index = new Index(indexFolder, settings, true);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Searching in the index
SearchResult result = index.Search("Einstein");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
