---
id: get-supported-file-formats
url: search/java/get-supported-file-formats
title: Get supported file formats
weight: 5
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
The [getSupportedFileTypes](https://apireference.groupdocs.com/search/java/com.groupdocs.search.results/FileType#getSupportedFileTypes()) method of the [FileType](https://apireference.groupdocs.com/search/java/com.groupdocs.search.results/FileType) class is used to obtain a list of supported file types.

An example of obtaining a list of supported file types is presented below.



```java
Iterable<FileType> supportedFileTypes = FileType.getSupportedFileTypes();
Iterator iterator = supportedFileTypes.iterator();      
while (iterator.hasNext()) {
    FileType fileType = (FileType)iterator.next();
    System.out.println(fileType.getExtension() + " - " + fileType.getDescription());
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
