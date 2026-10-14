---
id: evaluation-limitations-and-licensing
url: search/net/evaluation-limitations-and-licensing
title: Evaluation Limitations and Licensing
weight: 6
description: "This article describes the limitations of the evaluation version of GroupDocs.Search for .NET and how to apply a license or a metered license."
keywords: evaluation limitations, trial version, license, metered license, SetLicense, SetMeteredKey
productName: GroupDocs.Search for .NET
hideChildren: False
---
{{< alert style="info" >}}You can use GroupDocs.Search without a license. The usage and functionality are the same as in the licensed version, but the evaluation version has a few limitations.{{< /alert >}}

## Evaluation Version Limitations

You can easily download GroupDocs.Search for evaluation. The evaluation download is the same as the purchased download. The evaluation version simply becomes licensed when you add a few lines of code to apply the license. You will face the following limitations while using the API without a license.  

### Indexing Limitations

Following are the indexing limitations of the evaluation version of the **GroupDocs.Search API**:

*   The total number of indexed documents in one index or in all indexes of an index repository cannot exceed 100.
*   If adding documents would exceed this number, only the documents that fit into the limit are indexed.
*   All changed documents can be updated in an index during [Update](https://reference.groupdocs.com/search/net/groupdocs.search/index/update/).
*   New documents can be added to an index during update only while the total number of indexed documents is less than 100.
*   Indexes can be merged only if the total number of documents in the current index and in the merged indexes does not exceed 100. Otherwise, the [Merge](https://reference.groupdocs.com/search/net/groupdocs.search/index/merge/) method throws [LicenseRestrictionException](https://reference.groupdocs.com/search/net/groupdocs.search.exceptions/licenserestrictionexception/).

### Searching Limitations

Only the first 10 found documents can be retrieved from a search result. The [DocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/documentcount/) property returns the total number of found documents, but the [GetFoundDocument](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/getfounddocument/) method throws [LicenseRestrictionException](https://reference.groupdocs.com/search/net/groupdocs.search.exceptions/licenserestrictionexception/) for a document number of 10 or greater. Therefore, in the evaluation mode, a loop over found documents must not exceed 10 iterations:

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string query = "Einstein";

Index index = new Index(indexFolder);
SearchResult result = index.Search(query);
int count = Math.Min(result.DocumentCount, 10); // Only the first 10 found documents are available in the evaluation mode
for (int i = 0; i < count; i++)
{
    FoundDocument document = result.GetFoundDocument(i);
    Console.WriteLine(document.DocumentInfo.FilePath);
}
```

### Search Network Limitations

A [search network]({{< ref "search/net/developer-guide/advanced-usage/scaling/_index.md" >}}) can contain only 1 shard and 1 extractor. If the configuration of the network contains more shards or extractors, the methods of the indexer and searcher nodes throw [InvalidOperationException](https://learn.microsoft.com/dotnet/api/system.invalidoperationexception).

## Licensing 

The license file contains details such as the product name, number of developers it is licensed to, subscription expiry date and so on. It contains the digital signature, so don't modify the file. Even inadvertent addition of an extra line break into the file will invalidate it. You need to set a license before utilizing GroupDocs.Search API if you want to avoid its evaluation limitations. 

The license can be loaded from a file or stream object. A relative path to the license file is resolved against the current working directory of the process, which may differ from the folder of the application or of the GroupDocs.Search.dll file (for example, in a service or a web application). Therefore, it is recommended to specify the absolute path to the license file.

### Applying License from File

The code below will explain how to apply product license.

**C#**

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-search/GroupDocs.Search-for-.NET

// Initializing the license
License lic = new License();

// Setting the license from the specified file
lic.SetLicense(@"c:\Licenses\GroupDocs.Search.lic");
```

### Applying License from Stream

The following example shows how to load a license from a stream.

**C#**

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-search/GroupDocs.Search-for-.NET
using (FileStream fileStream = new FileStream(@"c:\Licenses\GroupDocs.Search.lic", FileMode.Open, FileAccess.Read))
{
    License lic = new License();
    lic.SetLicense(fileStream);
}
```

{{< alert style="info" >}}Calling License.SetLicense multiple times is not harmful but simply wastes processor time. If you are developing a Windows Forms or console application, call License.SetLicense in your startup code, before using GroupDocs.Search classes. When developing an ASP.NET application, you can call License.SetLicense from the Global.asax.cs (Global.asax.vb) file in the Application_Start protected method. It is called once when the application starts. Do not call License.SetLicense from within Page_Load methods since it means the license will be loaded every time a web page is loaded.{{< /alert >}}

### Applying Metered License

{{< alert style="info" >}}
You can also set [Metered](https://reference.groupdocs.com/search/net/groupdocs.search/metered/) license as an alternative to license file. It is a new licensing mechanism that will be used along with existing licensing method. It is useful for the customers who want to be billed based on the usage of the API features. For more details, please refer to [Metered Licensing FAQ](https://purchase.groupdocs.com/faqs/licensing/metered) section.
{{< /alert >}}

Here are the simple steps to use the `Metered` class.

1.  Create an instance of `Metered` class.
2.  Pass public and private keys to the `SetMeteredKey` method.
3.  Do processing (perform task).
4.  Call the static method `GetConsumptionQuantity` of the `Metered` class.
5.  It will return the consumption quantity (the amount of processed data in MBs) consumed so far.
6.  Call the static method `GetConsumptionCredit` of the `Metered` class (since version 19.5).
7.  It will return the credit that you have consumed so far.

Following is the sample code demonstrating how to use `Metered` class.

**C#**

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-search/GroupDocs.Search-for-.NET
string publicKey = "your-public-key"; // Your public license key
string privateKey = "your-private-key"; // Your private license key
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

Metered metered = new Metered();
metered.SetMeteredKey(publicKey, privateKey);

// Do indexing and searching in licensed mode
Index index = new Index(indexFolder);

// Adding documents to index
index.Add(documentsFolder);

// Searching
SearchResult result = index.Search("arbitrary");

// Get consumption quantity
decimal consumptionQuantity = GroupDocs.Search.Metered.GetConsumptionQuantity();

// Get consumption credit
decimal consumptionCredit = GroupDocs.Search.Metered.GetConsumptionCredit();
```
