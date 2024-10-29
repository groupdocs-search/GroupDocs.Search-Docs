---
id: extraction-in-separate-process
url: search/net/extraction-in-separate-process
title: Extraction in separate process
weight: 8.5
description: "This article describes how to minimize the situation of resource shortage in the indexing process"
keywords: extraction in separate process
productName: GroupDocs.Search for .NET
hideChildren: False
---
Extracting data from documents is the most resource-intensive operation of the indexing process. And in case of insufficient memory, an exception occurs that completely interrupts the process of indexing a group of documents. To prevent this situation, data can be extracted in a separate process. In this case, insufficient memory when extracting data from one document will lead to a failure in indexing only this document. Indexing of the remaining documents in the group will continue further.

Extracting data from documents in a separate process also solves another problem – the problem of immediate interruption of a document indexing process when the allocated time limit for indexing one document is exceeded.

To extract data from documents in a separate process, you need to configure the appropriate indexing options, and also create a simple console application project that the GroupDocs.Search library will run in a separate process as a data extraction service. The console application project for data extraction must have the GroupDocs.Search library as a dependency, as does the main project using the library. The console application project code is presented in the following listing.

**C#**

```csharp
public class Program
{
    static void Main(string[] args)
    {
        using (var host = new GroupDocs.Search.ExtractionHost(args))
        {
            host.Run();
        }
    }
}
```

An example of setting indexing options for data extraction in a separate process is shown in the listing below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";

// Getting the path to the console application location
string assemblyPath = typeof(GroupDocs.Search.Extraction.Program).Assembly.Location;

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Setting indexing options for data extraction in a separate process
IndexingOptions options = new IndexingOptions();
options.SeparateProcessOptions.ExtractInSeparateProcess = true;
options.SeparateProcessOptions.AssemblyPath = assemblyPath;
options.SeparateProcessOptions.Timeout = new TimeSpan(0, 1, 0);

// Indexing documents from the specified folder
index.Add(documentFolder, options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
