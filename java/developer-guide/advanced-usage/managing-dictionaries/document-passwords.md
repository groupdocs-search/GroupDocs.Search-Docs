---
id: document-passwords
url: search/java/document-passwords
title: Document passwords
weight: 4
description: "This article gives the knowledge of the API methods which can be used to perform operations about document passwords or password dictionary using Java."
keywords: document passwords, password dictionary
productName: GroupDocs.Search for Java
hideChildren: False
---
The [PasswordDictionary](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/PasswordDictionary) class is designed to store passwords for documents to be indexed. Information on indexing password protected documents is presented on the [Indexing password protected documents]({{< ref "search/java/developer-guide/advanced-usage/indexing/indexing-password-protected-documents.md" >}}) page.

To get the number of passwords in the dictionary, use the [getCount](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/PasswordDictionary#getCount()) method.

To get the password for a document from the dictionary, the [getPassword](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/PasswordDictionary#getPassword(java.lang.String)) method is used, passing the path to the document as an argument.

The [clear](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/PasswordDictionary#clear()) method is used to remove all passwords from the dictionary.

To check for the presence of a password in the dictionary for the specified document, the [contains](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/PasswordDictionary#contains(java.lang.String)) method is used.

To add a password to the dictionary, use the [add](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/PasswordDictionary#add(java.lang.String,%20java.lang.String)) method. The key is the path to the document.

To remove a password from the dictionary, use the [remove](https://apireference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/PasswordDictionary#remove(java.lang.String)) method.

The following example demonstrates the use of methods of the password dictionary.



```java
String indexFolder = "c:\\MyIndex\\";
 
// Creating an index from in specified folder
Index index = new Index(indexFolder);
 
if (index.getDictionaries().getDocumentPasswords().getCount() > 0) {
    // Removing all passwords from the dictionary
    index.getDictionaries().getDocumentPasswords().clear();
}
 
String path = new File("c:\\MyIndex\\Protected.pdf").getAbsolutePath();
if (index.getDictionaries().getDocumentPasswords().contains(path)) {
    // Getting a password for a document
    String password = index.getDictionaries().getDocumentPasswords().getPassword(path);
    System.out.println(path);
    System.out.println("\tPassword: " + password);
 
    // Deleting the password from the dictionary
    index.getDictionaries().getDocumentPasswords().remove(path);
}
 
// Adding a password for a document
index.getDictionaries().getDocumentPasswords().add(path, "123456");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
