---
id: document-passwords
url: search/net/document-passwords
title: Document passwords
weight: 4
description: "This article describes the API for managing the password dictionary that stores passwords of documents to be indexed."
keywords: document passwords, password dictionary
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [PasswordDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/passworddictionary/) class is designed to store passwords for documents to be indexed. The password dictionary of an index is available through the [DocumentPasswords](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/documentpasswords/) property of the [Index.Dictionaries](https://reference.groupdocs.com/search/net/groupdocs.search/index/dictionaries/) object. Information on indexing password protected documents is presented on the [Indexing password protected documents]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-password-protected-documents.md" >}}) page.

The password dictionary can be changed at any time. A password is stored with a key, which is the full path of a document file or, for a document added from a stream or a structure, the document key.

To get the number of passwords in the dictionary, use the [Count](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/passworddictionary/count/) property.

To get the password for a document from the dictionary, the [GetPassword](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/passworddictionary/getpassword/) method is used, passing the full path or the key of the document as an argument.

The [Clear](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/passworddictionary/clear/) method is used to remove all passwords from the dictionary.

To check for the presence of a password in the dictionary for the specified document, the [Contains](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/passworddictionary/contains/) method is used.

To add a password to the dictionary, use the [Add](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/passworddictionary/add/) method. The first parameter is the full path or the key of the document. If the dictionary already contains a password for the document, the method throws [ArgumentException](https://learn.microsoft.com/dotnet/api/system.argumentexception); remove the existing password first.

To remove a password from the dictionary, use the [Remove](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/passworddictionary/remove/) method.

To export passwords to a file, use the [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) method.

To import passwords from a file, use the [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) method. The imported passwords are added to the dictionary, so the dictionary must not already contain passwords for the same documents; otherwise, the import fails. Errors of exporting and importing are reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the password dictionary.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating or opening an index in the specified folder
Index index = new Index(indexFolder);

string path = Path.GetFullPath(@"c:\MyDocuments\Protected.pdf");
if (index.Dictionaries.DocumentPasswords.Contains(path))
{
    // Getting a password for a document
    string password = index.Dictionaries.DocumentPasswords.GetPassword(path);
    Console.WriteLine(path);
    Console.WriteLine("\tPassword: " + password);

    // Deleting the password from the dictionary
    index.Dictionaries.DocumentPasswords.Remove(path);
}

if (index.Dictionaries.DocumentPasswords.Count > 0)
{
    // Removing all passwords from the dictionary
    index.Dictionaries.DocumentPasswords.Clear();
}

// Adding a password for a document
index.Dictionaries.DocumentPasswords.Add(path, "123456");

// Exporting passwords to a file
index.Dictionaries.DocumentPasswords.ExportDictionary(@"c:\Passwords.dat");

// Removing all passwords and importing them from the file
index.Dictionaries.DocumentPasswords.Clear();
index.Dictionaries.DocumentPasswords.ImportDictionary(@"c:\Passwords.dat");

// Indexing documents; the passwords from the dictionary are used to open protected documents
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
