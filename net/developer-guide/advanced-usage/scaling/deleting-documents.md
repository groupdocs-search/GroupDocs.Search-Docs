---
id: deleting-documents
url: search/net/deleting-documents
title: Deleting documents
weight: 50
description: "This page contains information about deleting documents in the search network."
keywords: search network deleting documents, distributed index removing documents, delete documents in search network
productName: GroupDocs.Search for .NET
hideChildren: False
---
Documents can be deleted from the search network using the [Delete](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/delete/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class.

To delete documents, you must pass an array of document keys, as well as a delete options object, to the [Delete](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/delete/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) method.

An asynchronous delete flag is available as an option. In the case of asynchronous deletion, you can track the completion of the operation by subscribing to the [DeletionCompleted](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/deletioncompleted/) event.

The following code example demonstrates how to delete documents from the search network.

**C#**

```csharp
Console.WriteLine();
Console.WriteLine("Deleting documents");

string[] fileNames = new string[filePaths.Length];
for (int i = 0; i < filePaths.Length; i++)
{
    string filePath = filePaths[i];
    string fileName = Path.GetFileName(filePath);
    fileNames[i] = fileName;
}

Indexer indexer = node.Indexer;

DeleteOptions options = new DeleteOptions();
indexer.Delete(fileNames, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
