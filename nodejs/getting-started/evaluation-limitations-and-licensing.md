---
id: evaluation-limitations-and-licensing
url: search/nodejs-java/evaluation-limitations-and-licensing
title: Evaluation Limitations and Licensing
weight: 6
description: "Free search api version is available to evaluate the API which will be similar as licensed but with few limitations."
keywords: free search api, search api
productName: GroupDocs.Search for Node.js via Java
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

```javascriptscript
const licensePath = "path to the .lic file";
const license = new groupdocs.search.License()
license.setLicense(licensePath);
```

### Applying License from Stream

The following example shows how to load a license from a stream.

```javascriptscript
const java = require('java');
let InputStream = java.import('java.io.FileInputStream');

const licensePath = "path to the .lic file";
const stream = new InputStream(licensePath);
const license = new groupdocs.search.License();
await license.setLicense(stream);
```
