---
id: document-passwords
url: search/nodejs-java/document-passwords
title: Document passwords
weight: 4
description: "This article gives the knowledge of the API methods which can be used to perform operations about document passwords or password dictionary using Java."
keywords: document passwords, password dictionary
productName: GroupDocs.Search for Node.js via Java
hideChildren: False
---
The [PasswordDictionary](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/PasswordDictionary) class is designed to store passwords for documents to be indexed. Information on indexing password protected documents is presented on the [Indexing password protected documents]({{< ref "search/nodejs-java/developer-guide/advanced-usage/indexing/indexing-password-protected-documents.md" >}}) page.

To get the number of passwords in the dictionary, use the [getCount](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/PasswordDictionary#getCount()) method.

To get the password for a document from the dictionary, the [getPassword](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/PasswordDictionary#getPassword(java.lang.String)) method is used, passing the path to the document as an argument.

The [clear](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/PasswordDictionary#clear()) method is used to remove all passwords from the dictionary.

To check for the presence of a password in the dictionary for the specified document, the [contains](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/PasswordDictionary#contains(java.lang.String)) method is used.

To add a password to the dictionary, use the [add](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/PasswordDictionary#add(java.lang.String,%20java.lang.String)) method. The key is the path to the document.

To remove a password from the dictionary, use the [remove](https://reference.groupdocs.com/search/nodejs-java/com.groupdocs.search.dictionaries/PasswordDictionary#remove(java.lang.String)) method.

The following example demonstrates the use of methods of the password dictionary.

```javascript
const indexFolder = 'c:/MyIndex/';
const documentsFolder = 'c:/MyDocuments/';

// Creating an index from in specified folder
const index = new groupdocs.search.Index(indexFolder);

if (index.getDictionaries().getDocumentPasswords().getCount() > 0) {
  // Removing all passwords from the dictionary
  index.getDictionaries().getDocumentPasswords().clear();
}

const absolutePath = path.resolve(Utils.PasswordProtectedDocumentsPath + 'English.docx');
index.getDictionaries().getDocumentPasswords().add(absolutePath, '123456');

if (index.getDictionaries().getDocumentPasswords().contains(absolutePath)) {
  // Getting a password for a document
  const password = index.getDictionaries().getDocumentPasswords().getPassword(absolutePath);
  console.log(absolutePath);
  console.log('\tPassword: ' + password);

  // Deleting the password from the dictionary
  index.getDictionaries().getDocumentPasswords().remove(absolutePath);
}

// Adding document passwords to the dictionary
index
  .getDictionaries()
  .getDocumentPasswords()
  .add(Utils.PasswordProtectedDocumentsPath + 'English.docx', '123456');
index
  .getDictionaries()
  .getDocumentPasswords()
  .add(Utils.PasswordProtectedDocumentsPath + 'Lorem ipsum.docx', '123456');

// Indexing documents from the specified folder
// Passwords will be automatically retrieved from the dictionary when necessary
index.add(documentsFolder);

// Searching in the index
const query = 'ipsum OR increasing';
const result = index.search(query);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
