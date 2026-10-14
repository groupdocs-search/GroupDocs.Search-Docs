---
id: extraction-in-separate-process
url: search/net/extraction-in-separate-process
title: Extraction in separate process
weight: 11
description: "This article describes how to extract data from documents in a separate process to protect the indexing process from memory shortage and long extraction."
keywords: extraction in separate process, ExtractionHost, SeparateProcessOptions, out of memory, extraction timeout
productName: GroupDocs.Search for .NET
hideChildren: False
---
Extracting data from documents is the most resource-intensive operation of the indexing process. In case of insufficient memory, an exception occurs that completely interrupts the process of indexing a group of documents. To prevent this situation, data can be extracted in a separate process. In this case, insufficient memory when extracting data from one document will lead to a failure in indexing only this document. Indexing of the remaining documents in the group will continue.

Extracting data from documents in a separate process also solves another problem: a document whose extraction exceeds the allocated time limit is interrupted at once.

To extract data from documents in a separate process, you need to configure the appropriate indexing options and create a simple console application project that the GroupDocs.Search library will run in a separate process as a data extraction service. The console application project for data extraction must have the GroupDocs.Search library as a dependency, as does the main project using the library. The console application project code is presented in the following listing.

**C#**

```csharp
namespace ExtractionService
{
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
}
```

The [ExtractionHost](https://reference.groupdocs.com/search/net/groupdocs.search/extractionhost/) class receives the name of the communication channel from the command line arguments, so the arguments must be passed to its constructor unchanged. The [Run](https://reference.groupdocs.com/search/net/groupdocs.search/extractionhost/run/) method processes extraction requests until the main process stops the service. The license set in the main process is passed to the extraction process automatically. Requests for document passwords and extraction errors are passed to the main process.

Custom text extractors added to the index settings are not passed to the extraction process. To use [custom text extractors]({{< ref "search/net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}), add them to the [CustomExtractors](https://reference.groupdocs.com/search/net/groupdocs.search/extractionhost/customextractors/) collection of the host before calling the [Run](https://reference.groupdocs.com/search/net/groupdocs.search/extractionhost/run/) method, for example: `host.CustomExtractors.Add(new LogExtractor());`.

The options for extraction in a separate process are specified in the [SeparateProcessOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/) object available through the [IndexingOptions.SeparateProcessOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/separateprocessoptions/) or [UpdateOptions.SeparateProcessOptions](https://reference.groupdocs.com/search/net/groupdocs.search.options/updateoptions/separateprocessoptions/) property:

*   [ExtractInSeparateProcess](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/extractinseparateprocess/) enables extraction in a separate process. The default value is false. Note that in this mode, data are extracted in only one thread: the number of threads specified in the [Threads](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/threads/) property is ignored.
*   [AssemblyPath](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/assemblypath/) specifies the path to the assembly of the console application.
*   [Timeout](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/timeout/) specifies the maximum time to wait for the result of data extraction from one document. The default value is 10 minutes.
*   [UseDotnetToStartProcess](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/usedotnettostartprocess/) specifies whether the process is started with the `dotnet` command, which runs the assembly specified by [AssemblyPath](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/assemblypath/). The default value is true. Set it to false to start a self-contained application; in this case, [AssemblyPath](https://reference.groupdocs.com/search/net/groupdocs.search.options/separateprocessoptions/assemblypath/) must specify the executable file. On .NET Framework, `dotnet` is never used, and the executable file is started directly.

An example of setting indexing options for data extraction in a separate process is shown in the listing below. In this example, the main project references the console application project, so the path to the console application can be obtained from its type; otherwise, specify the path explicitly.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";

// Getting the path to the console application location
string assemblyPath = typeof(ExtractionService.Program).Assembly.Location;

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
