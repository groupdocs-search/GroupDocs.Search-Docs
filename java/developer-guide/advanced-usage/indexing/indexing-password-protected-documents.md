---
id: indexing-password-protected-documents
url: search/java/indexing-password-protected-documents
title: Indexing password protected documents
weight: 13
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
This page describes how to perform indexing of password protected documents.

## Indexing using the password dictionary

To perform indexing of password protected documents using a password dictionary, you must add passwords for all protected documents to the dictionary before indexing. To add a document password to the dictionary, you must specify the full path to the document as a key and the actual password to the document. For more information about managing the password dictionary see the [Document passwords]({{< ref "search/java/developer-guide/advanced-usage/managing-dictionaries/document-passwords.md" >}}) page in the [Managing dictionaries]({{< ref "search/java/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

The password dictionary is stored on disk in encrypted form. However, very simple encryption is used, so if you want to protect the passwords themselves, it is better to use the [PasswordRequired](https://reference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#PasswordRequired) event to set passwords.

The following example demonstrates how to perform indexing of password protected documents using a password dictionary.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Adding document passwords to the dictionary
String path = new File("C:\\MyDocuments\\ProtectedDocument.pdf").getAbsolutePath();
index.getDictionaries().getDocumentPasswords().add(path, "123456");
// ...
 
// Indexing documents from the specified folder
// Passwords will be automatically retrieved from the dictionary when necessary
index.add(documentsFolder);
```

## Indexing using the PasswordRequired event

The password for the protected document can also be provided when the [PasswordRequired](https://reference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#PasswordRequired) event occurs. This method is more secure, however, it requires matching the indexed documents and their passwords in the event handler.

Both of the described methods for providing document passwords can be used together. The index first checks for a document password in the password dictionary, and then, if the password is absent, raises the [PasswordRequired](https://reference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#PasswordRequired) event. If the password is not provided or an incorrect password is provided, the index will raise the [ErrorOccurred](https://reference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#ErrorOccurred) event with the corresponding error message and the document will not be indexed. You can learn more about index events on the page [Search index events]({{< ref "search/java/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}).

The following example shows how to provide password for a document using the [PasswordRequired](https://reference.groupdocs.com/search/java/com.groupdocs.search.events/EventHub#PasswordRequired) event.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.getEvents().PasswordRequired.add(new EventHandler<PasswordRequiredEventArgs>() {
    public void invoke(Object sender, PasswordRequiredEventArgs args) {
        if (args.getDocumentFullPath().endsWith("ProtectedDocument.pdf")) {
            args.setPassword("123456"); // Providing password for the file 'ProtectedDocument.pdf'
        }
    }
});
 
// Indexing documents from the specified folder
index.add(documentsFolder);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
