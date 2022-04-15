---
id: evaluation-limitations-and-licensing
url: search/java/evaluation-limitations-and-licensing
title: Evaluation Limitations and Licensing
weight: 6
description: "Free search api version is available to evaluate the API which will be similar as licensed but with few limitations."
keywords: free search api, search api
productName: GroupDocs.Search for Java
hideChildren: False
---
{{< alert style="info" >}}You can use GroupDocs.Search without the license. The usage and functionalities are pretty much same as the licensed one but you will face few limitations while using the non-licensed API.{{< /alert >}}

## Evaluation Version Limitations

You can easily download GroupDocs.Search for evaluation. The evaluation download is the same as the purchased download. The evaluation version simply becomes licensed when you add a few lines of code to apply the license. You will face following limitations while using the API without the license.  

### Indexing Limitations

Following are the indexing limitations user will face while using the trial version of the **GroupDocs.Search API**:

*   Total number of indexed documents in one index or in several indexes for index repository cannot exceed 100 files.
*   Indexing stops when number of indexed documents in an index or in all indexes in index repository becomes 100.
*   All changed files can be updated in an index during Update.
*   New files can only be added to an index during update if total number of indexed documents is less than 100.

### Searching Limitations

Only first 10 documents returned in search result.

## Licensing 

The license file contains details such as the product name, number of developers it is licensed to, subscription expiry date and so on. It contains the digital signature, so don't modify the file. Even inadvertent addition of an extra line break into the file will invalidate it. You need to set a license before utilizing GroupDocs.Search API if you want to avoid its evaluation limitations. 

The license can be loaded from a file or stream object. The easiest way to set a license is to put the license file in the same folder as the GroupDocs.Search.dll file and specify the file name, without a path, as shown in the examples below.

### Applying License from File

The code below will explain how to apply product license.



```java
// For complete examples and data files, please go to https://github.com/groupdocs-search/GroupDocs.Search-for-Java

//initialize License
License lic = new License();

//Set license
lic.setLicense("GroupDocs.Search.lic");
```

### Applying License from Stream

The following example shows how to load a license from a stream.



```java
// For complete examples and data files, please go to https://github.com/groupdocs-search/GroupDocs.Search-for-Java
Path fileLocation = Paths.get("GroupDocs.Search.lic");
 
try {
    byte[] data = Files.readAllBytes(fileLocation);
    ByteArrayInputStream inputStream = new ByteArrayInputStream(data);
 
    License lic = new License();
    lic.setLicense(inputStream);
} catch (IOException e) {
    System.out.println(e.getMessage());
}
```

### Applying Metered License

{{< alert style="info" >}}
You can also set Metered license as an alternative to license file. It is a new licensing mechanism that will be used along with existing licensing method. It is useful for the customers who want to be billed based on the usage of the API features. For more details, please refer to [Metered Licensing FAQ](https://purchase.groupdocs.com/faqs/licensing/metered) section.
{{< /alert >}}

Here are the simple steps to use the **`Metered`**class.

1.  Create an instance of **`Metered` **class.
2.  Pass public & private keys to **s`etMeteredKey`** method.
3.  Do processing (perform task).
4.  Call method **g`etConsumptionQuantity` **of the **`Metered` **class.
5.  It will return the amount/quantity of API requests that you have consumed so far.
6.  Call method **g`etConsumptionCredit` **of the **`Metered` **class (Since version 19.5).
7.  It will return the credit that you have consumed so far.

Following is the sample code demonstrating how to use **`Metered`** class.



```java
// For complete examples and data files, please go to https://github.com/groupdocs-search/GroupDocs.Search-for-Java
 
String indexPath = "c:\\MyIndex";
String documentsPath = "c:\\MyDocuments";
String PublicKey = ""; // Your public license key
String PrivateKey = ""; // Your private license key
Metered metered = new Metered();
 
try {
    metered.setMeteredKey(PublicKey, PrivateKey);
} catch (Exception e) {
    System.out.println(e.getMessage());
}
 
// Do indexing and searching in licensed mode 
Index index = new Index(indexPath);
 
// Adding documents to index
index.add(documentsPath);
 
// Searching
SearchResult result = index.search("arbitrary");
 
// Get consumption quantity
try {
    double consumptionQuantitiy = Metered.getConsumptionQuantity();
} catch (Exception e) {
    System.out.println(e.getMessage());
}
 
// Get consumption credit
try {
    double consumptionCredit = Metered.getConsumptionCredit();
} catch (Exception e) {
    System.out.println(e.getMessage());
}
```
