---
id: reverse-image-search
url: search/python-net/reverse-image-search
title: Reverse image search
weight: 17.5
description: "This article describes the reverse image search, which makes it possible to search for similar images in ZIP archives, various documents, and individual files."
keywords: Reverse image search
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Reverse image search is a search for images that are similar to a given reference image.

In the GroupDocs.Search index, reverse image search allows you to search for similar images in ZIP archives, various documents, and individual files.
The image search is performed by comparing the perceptual hash of the reference image with the hashes of the images in the index. The idea of a perceptual hash is that for very similar images it has a value with a minimum number of different bits, and for very different images it gives a large number of different bits.

In the GroupDocs.Search engine, reverse image search, like full-text search, consists of two stages: the indexing stage and the actual search stage.

To search for images in the index, you must enable at least one of the flags in the options at the indexing stage:
* enabled_for_separate_images is for indexing images in separate files.
* enabled_for_embedded_images is for indexing images embedded in various documents.
* enabled_for_container_item_images is for indexing images that are elements of container documents, such as ZIP archives, OST/PST storages.
For more information, see the [Indexing options]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/indexing-options.md" >}}) page.

The following options are available during the search stage:
* hash_differences sets the maximum number of different bits in the hashes of found images. The hash is 64 bits long, so this value ranges from 0 to 64.
* max_result_count sets the maximum number of found images.
* search_document_filter sets the filter for found documents.
For more information, see the [Image search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/image-search-options.md" >}}) page.

A reference image is created with one of the overloads of the [SearchImage.create](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/searchimage/create/) method:
* create(file_path) creates a reference image from an image file.
* create(stream) creates a reference image from a stream containing an image, for example, a binary file object.
* create(file_path, image_index) and create(stream, image_index) create a reference image from the specified frame (zero-based index) of a multipage image, for example a multipage TIFF image.

The search result is returned as an instance of the [ImageSearchResult](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/imagesearchresult/) class. The [image_count](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/imagesearchresult/image_count/) property returns the number of found images, and the [get_found_image](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/imagesearchresult/get_found_image/) method returns a found image by its index. The found images with fewer different bits in the hash are returned first. Each found image is represented by an instance of the [FoundImageFrame](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/foundimageframe/) class, which has the following properties:
* [document_info](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/foundimageframe/document_info/) returns information about the document containing the found image.
* [image_index](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/foundimageframe/image_index/) returns the ordinal number of the image in the document.
* [frame_index](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/foundimageframe/frame_index/) returns the index of the frame in the image.
* [hash_differences](https://reference.groupdocs.com/search/python-net/groupdocs.search.results/foundimageframe/hash_differences/) returns the number of different bits in the hash of the found image.

The following code example demonstrates all stages of the reverse image search:

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.common import SearchImage
from groupdocs.search.options import ImageSearchOptions, IndexingOptions, SearchDocumentFilter

# Creating an index
index = Index("./index")

# Setting the image indexing options
indexing_options = IndexingOptions()
indexing_options.image_indexing_options.enabled_for_container_item_images = True
indexing_options.image_indexing_options.enabled_for_embedded_images = True
indexing_options.image_indexing_options.enabled_for_separate_images = True

# Indexing documents in a document folder
index.add("./documents", indexing_options)

# Setting the image search options
image_search_options = ImageSearchOptions()
image_search_options.hash_differences = 10
image_search_options.max_result_count = 100
image_search_options.search_document_filter = SearchDocumentFilter.create_file_extension(".zip", ".png", ".jpg")

# Creating a reference image for search
search_image = SearchImage.create("./image.png")

# Searching in the index
result = index.search(search_image, image_search_options)

print("Images found: " + str(result.image_count))
for i in range(result.image_count):
    image = result.get_found_image(i)
    document_info = image.document_info
    name = os.path.basename(document_info.file_path)
    if document_info.inner_path:
        name += " / " + document_info.inner_path  # An item of a container document, such as a ZIP archive
    print(name + ", image " + str(image.image_index) + ", frame " + str(image.frame_index) + ", different bits: " + str(image.hash_differences))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/travel-report.docx](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/reverse-image-search/documents/travel-report.docx), [documents/holiday-photos.zip](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/reverse-image-search/documents/holiday-photos.zip), [documents/sunset.png](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/reverse-image-search/documents/sunset.png), [documents/sunset-brighter.png](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/reverse-image-search/documents/sunset-brighter.png), [documents/checkerboard.png](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/reverse-image-search/documents/checkerboard.png)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "image.png" >}}
{{< tab-text >}}
The following sample file is used in this example: [image.png](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/reverse-image-search/image.png)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "reverse-image-search.txt" >}}  
```text
Images found: 3
holiday-photos.zip / photos\sunset.png, image 0, frame 0, different bits: 0
sunset.png, image 0, frame 0, different bits: 0
sunset-brighter.png, image 0, frame 0, different bits: 4
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/reverse-image-search/reverse_image_search/reverse-image-search.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
