---
id: document-passwords
url: search/net/document-passwords
title: Document passwords
weight: 4
description: "This article gives the knowledge of the API methods which can be used to perform operations about document passwords or password dictionary."
keywords: document passwords, password dictionary
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [PasswordDictionary](https://reference.groupdocs.com/net/search/groupdocs.search.dictionaries/passworddictionary) class is designed to store passwords for documents to be indexed. Information on indexing password protected documents is presented on the [Indexing password protected documents]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-password-protected-documents.md" >}}) page.

To get the number of passwords in the dictionary, use the [Count](https://reference.groupdocs.com/net/search/groupdocs.search.dictionaries/passworddictionary/properties/count) property.

To get the password for a document from the dictionary, the [GetPassword](https://reference.groupdocs.com/net/search/groupdocs.search.dictionaries/passworddictionary/methods/getpassword) method is used, passing the path to the document as an argument.

The [Clear](https://reference.groupdocs.com/net/search/groupdocs.search.dictionaries/passworddictionary/methods/clear) method is used to remove all passwords from the dictionary.

To check for the presence of a password in the dictionary for the specified document, the [Contains](https://reference.groupdocs.com/net/search/groupdocs.search.dictionaries/passworddictionary/methods/contains) method is used.

To add a password to the dictionary, use the [Add](https://reference.groupdocs.com/net/search/groupdocs.search.dictionaries/passworddictionary/methods/add) method. The key is the path to the document.

To remove a password from the dictionary, use the [Remove](https://reference.groupdocs.com/net/search/groupdocs.search.dictionaries/passworddictionary/methods/remove) method.

The following example demonstrates the use of methods of the password dictionary.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
 
// Creating an index from in specified folder
Index index = new Index(indexFolder);
 
if (index.Dictionaries.DocumentPasswords.Count > 0)
{
    // Removing all passwords from the dictionary
    index.Dictionaries.DocumentPasswords.Clear();
}
 
string path = Path.GetFullPath(@"c:\MyIndex\Protected.pdf");
if (index.Dictionaries.DocumentPasswords.Contains(path))
{
    // Getting a password for a document
    string password = index.Dictionaries.DocumentPasswords.GetPassword(path);
    Console.WriteLine(path);
    Console.WriteLine("\tPassword: " + password);
 
    // Deleting the password from the dictionary
    index.Dictionaries.DocumentPasswords.Remove(path);
}
 
// Adding a password for a document
index.Dictionaries.DocumentPasswords.Add(path, "123456");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
