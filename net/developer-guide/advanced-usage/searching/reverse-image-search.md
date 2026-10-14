---
id: reverse-image-search
url: search/net/reverse-image-search
title: Reverse image search
weight: 17.5
description: "This article describes the reverse image search, which makes it possible to search for similar images in ZIP archives, various documents, and individual files."
keywords: Reverse image search
productName: GroupDocs.Search for .NET
hideChildren: False
---
Reverse image search is a search for images that are similar to a given reference image.

In the GroupDocs.Search index, reverse image search allows you to search for similar images in ZIP archives, various documents, and individual files.
The image search is performed by comparing the perceptual hash of the reference image with the hashes of the images in the index. The idea of a perceptual hash is that for very similar images it has a value with a minimum number of different bits, and for very different images it gives a large number of different bits.

In the GroupDocs.Search engine, reverse image search, like full-text search, consists of two stages: the indexing stage and the actual search stage.

To search for images in the index, you must enable at least one of the flags in the options at the indexing stage:
* EnabledForSeparateImages is for indexing images in separate files.
* EnabledForEmbeddedImages is for indexing images embedded in various documents.
* EnabledForContainerItemImages is for indexing images that are elements of container documents, such as ZIP archives, OST/PST storages.
For more information, see the [Indexing options]({{< ref "search/net/developer-guide/advanced-usage/indexing/indexing-options.md" >}}) page.

The following options are available during the search stage:
* HashDifferences sets the maximum number of different bits in the hashes of found images. The hash is 64 bits long, so this value ranges from 0 to 64.
* MaxResultCount sets the maximum number of found images.
* SearchDocumentFilter sets the filter for found documents.
For more information, see the [Image search options]({{< ref "search/net/developer-guide/advanced-usage/searching/image-search-options.md" >}}) page.

A reference image is created with one of the overloads of the [SearchImage.Create](https://reference.groupdocs.com/search/net/groupdocs.search.common/searchimage/create/) method:
* Create(string filePath) creates a reference image from an image file.
* Create(Stream stream) creates a reference image from a stream containing an image.
* Create(string filePath, int imageIndex) and Create(Stream stream, int imageIndex) create a reference image from the specified frame (zero-based index) of a multipage image, for example a multipage TIFF image.

The search result is returned as an instance of the [ImageSearchResult](https://reference.groupdocs.com/search/net/groupdocs.search.results/imagesearchresult/) class. The [ImageCount](https://reference.groupdocs.com/search/net/groupdocs.search.results/imagesearchresult/imagecount/) property returns the number of found images, and the [GetFoundImage](https://reference.groupdocs.com/search/net/groupdocs.search.results/imagesearchresult/getfoundimage/) method returns a found image by its index. The found images with fewer different bits in the hash are returned first. Each found image is represented by an instance of the [FoundImageFrame](https://reference.groupdocs.com/search/net/groupdocs.search.results/foundimageframe/) class, which has the following properties:
* [DocumentInfo](https://reference.groupdocs.com/search/net/groupdocs.search.results/foundimageframe/documentinfo/) returns information about the document containing the found image.
* [ImageIndex](https://reference.groupdocs.com/search/net/groupdocs.search.results/foundimageframe/imageindex/) returns the ordinal number of the image in the document.
* [FrameIndex](https://reference.groupdocs.com/search/net/groupdocs.search.results/foundimageframe/frameindex/) returns the index of the frame in the image.
* [HashDifferences](https://reference.groupdocs.com/search/net/groupdocs.search.results/foundimageframe/hashdifferences/) returns the number of different bits in the hash of the found image.

The following code example demonstrates all stages of the reverse image search:

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";

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
    Console.WriteLine(image.DocumentInfo.FilePath + ", image " + image.ImageIndex + ", frame " + image.FrameIndex + ", different bits: " + image.HashDifferences);
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
