---
id: indexing-password-protected-documents
url: search/net/indexing-password-protected-documents
title: Indexing password protected documents
weight: 16
description: "This article explains how to index password protected documents using the password dictionary or the PasswordRequired event."
keywords: indexing of password protected documents, document passwords, password dictionary, PasswordRequired
productName: GroupDocs.Search for .NET
hideChildren: False
---
This page describes how to perform indexing of password protected documents.

## Indexing using the password dictionary

To perform indexing of password protected documents using a password dictionary, you must add passwords for all protected documents to the dictionary before indexing. To add a document password to the dictionary, you must specify the full path to the document (or the document key for a document added from a stream) as a key and the actual password to the document. The password dictionary can be changed at any time. For more information about managing the password dictionary see the [Document passwords]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/document-passwords.md" >}}) page in the [Managing dictionaries]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

{{< alert style="info" >}}The passwords are not protected in the index, in an exported file or in the messages of a search network: anyone who can read the folder of the index, the exported file or the traffic of the network can read them. Keep the folder of an index that has passwords, and the exported files, where only trusted users can read them. To keep passwords out of the index, provide them with the [PasswordRequired](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/passwordrequired/) event instead (see [Indexing using the PasswordRequired event](#indexing-using-the-passwordrequired-event) below).{{< /alert >}}

The following example demonstrates how to perform indexing of password protected documents using a password dictionary.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Adding document passwords to the dictionary
string key = Path.GetFullPath(@"C:\MyDocuments\ProtectedDocument.pdf");
index.Dictionaries.DocumentPasswords.Add(key, "123456");
// ...
 
// Indexing documents from the specified folder
// Passwords will be automatically retrieved from the dictionary when necessary
index.Add(documentsFolder);
```

## Indexing using the PasswordRequired event

The password for the protected document can also be provided when the **[PasswordRequired](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/passwordrequired/)** event occurs. This method is more secure; however, it requires matching the indexed documents and their passwords in the event handler.

Both of the described methods for providing document passwords can be used together. The index first checks for a document password in the password dictionary, and then, if the password is absent, raises the **[PasswordRequired](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/passwordrequired/)** event. If the password is not provided or an incorrect password is provided, the index will raise the **[ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/)** event with the corresponding error message and the document will not be indexed. You can learn more about index events on the page [Search index events]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}). Since version 26.11, the passwords of the dictionary are used in the same way for the documents that are [extracted in a separate process]({{< ref "search/net/developer-guide/advanced-usage/indexing/extraction-in-separate-process.md" >}}); before, the separate process got only the passwords of the PasswordRequired event.

The following example shows how to provide password for a document using the **[PasswordRequired](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/passwordrequired/)** event.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Subscribing to the event
index.Events.PasswordRequired += (sender, args) =>
{
    if (args.DocumentFullPath.EndsWith("ProtectedDocument.pdf", StringComparison.InvariantCultureIgnoreCase))
    {
        args.Password = "123456"; // Providing password for the file 'ProtectedDocument.pdf'
    }
};
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
