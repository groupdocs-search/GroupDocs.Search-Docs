---
id: reverse-image-search
url: search/net/reverse-image-search
title: Reverse image search
weight: 17.5
description: "This article gives knowledge about the reverse image search, which makes it possible to search for similar images in ZIP archives, various documents, and individual files."
keywords: Reverse image search
productName: GroupDocs.Search for .NET
hideChildren: False
---
Reverse image search is a search for images that are similar to a given reference image.

In the GroupDocs.Search index, reverse image search allows you to search for similar images in ZIP archives, various documents, and individual files.
The image search is performed by comparing the perceptual hash of the reference image with the hashes of the images in the index. The idea of ​​a perceptual hash is that for very similar images it has a value with a minimum number of different bits, and for very different images it gives a large number of different bits.

In the GroupDocs.Search engine, reverse image search, like full-text search, consists of two stages: the indexing stage and the actual search stage.

To search for images in the index, you must enable at least one of the flags in the options at the indexing stage:
* EnabledForSeparateImages is for indexing images in separate files.
* EnabledForEmbeddedImages is for indexing images embedded in various documents.
* EnabledForContainerItemImages is for indexing images that are elements of container documents, such as ZIP archives, OST/PST storages.
For more information, see the [Indexing options]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-options.md" >}}) page.

The following options are available during the search stage:
* HashDifferences sets the maximum number of different bits in the hashes of found images. This value ranges from 0 to 32.
* MaxResultCount sets the maximum number of found images.
* SearchDocumentFilter sets the filter for found documents.
For more information, see the [Image search options]({{< ref "search/net/developer-guide/advanced-usage/searching/image-search-options.md" >}}) page.

The following code example demonstrates all stages of the reverse image search:

**C#**

```csharp
string indexFolder = @"c:\MyIndex";
string documentFolder = @"c:\MyDocuments";

// Creating an index
Index index = new Index(indexFolder);

// Setting the image indexing options
IndexingOptions indexingOptions = new IndexingOptions();
indexingOptions.ImageIndexingOptions.EnabledForContainerItemImages = true;
indexingOptions.ImageIndexingOptions.EnabledForEmbeddedImages = true;
indexingOptions.ImageIndexingOptions.EnabledForSeparateImages = true;

// Indexing documents in a document folder
index.Add(documentFolder, indexingOptions);

// Setting the image search options
ImageSearchOptions imageSearchOptions = new ImageSearchOptions();
imageSearchOptions.HashDifferences = 10;
imageSearchOptions.MaxResultCount = 100;
imageSearchOptions.SearchDocumentFilter = SearchDocumentFilter.CreateFileExtension(".zip", ".png", ".jpg");

// Creating a reference image for search
SearchImage searchImage = SearchImage.Create(@"c:\MyDocuments\image.png");

// Searching in the index
ImageSearchResult result = index.Search(searchImage, imageSearchOptions);

Console.WriteLine("Images found: " + result.ImageCount);
for (int i = 0; i < result.ImageCount; i++)
{
    FoundImageFrame image = result.GetFoundImage(i);
    Console.WriteLine(image.DocumentInfo.ToString());
}
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
