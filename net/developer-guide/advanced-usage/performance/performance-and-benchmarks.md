---
id: performance-and-benchmarks
url: search/net/performance-and-benchmarks
title: Performance and benchmarks
weight: 1
description: "Real performance of GroupDocs.Search for .NET on one million documents: indexing time, index size, optimization and the time of 20 kinds of search, and how to measure it on your own documents."
keywords: search benchmark, full-text search performance, indexing speed, search latency, index size on disk, one million documents, govdocs1, .NET search benchmark
productName: GroupDocs.Search for .NET
hideChildren: False
---
This article shows the performance of GroupDocs.Search for .NET 26.11 on one million real documents and an ordinary desktop computer: how long the indexing takes, how large the index grows, how long the optimization takes and how fast each kind of search is. All numbers were measured by the open-source [GroupDocs.Search benchmarks](https://github.com/groupdocs-search/full-text-search-benchmark-net) application, which you can run on your own documents and hardware to plan your project.

## Results at a glance

| Operation | Result |
| --- | --- |
| Indexing of 919,896 documents | 3 h 48 min, 67 documents per second |
| Index after the indexing | 60.2 GB, 727 segments, 285.6 million unique terms |
| Optimization | 12 h 24 min: 727 segments merged into 28, 49.5 GB |
| Opening of the optimized index | 9.8 s |
| 17 of 20 kinds of search, repeated runs | 35 to 539 ms, median 158 ms |
| 17 of 20 kinds of search, first run | 52 to 591 ms |
| The heaviest queries, repeated runs | Numeric range 8.5 s, regular expression 26.5 s, date range of a year 87.2 s |

The rest of the article describes the conditions of the measurement, the details of every stage and the factors that the results depend on.

## Test environment

| Component | Value |
| --- | --- |
| Processor | Intel Core i5-11500: 6 cores, 12 logical processors |
| Memory | 32 GB |
| Disk | SATA SSD for the extracted data and the index |
| Operating system | Windows 10 (10.0.19045) |
| Runtime | .NET 10.0.14, x64, workstation garbage collector |
| Library | GroupDocs.Search 26.11, the build for .NET 10 |
| Index settings | The defaults: the text of the documents is not [stored]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}) in the index |
| Indexing | 4 threads ([Threads](https://reference.groupdocs.com/search/net/groupdocs.search.options/indexingoptions/threads/)); up to 10,000 documents or 512 MB of extracted data per call of [Add](https://reference.groupdocs.com/search/net/groupdocs.search/index/add/) |
| Search | [SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/) left at the default: as many threads as there are processor cores |

The benchmark ran with a license applied: in the [evaluation mode]({{< ref "search/net/getting-started/evaluation-limitations-and-licensing.md" >}}) the number of indexed and found documents is limited, so the results would not be representative.

## Documents

The documents are the [govdocs1](https://digitalcorpora.org/corpora/files/) corpus of Digital Corpora: nearly one million files collected from web servers of the US government. It contains PDF, HTML, Word, Excel, PowerPoint, text, XML, CSV, images and other formats, including many spreadsheets and data files with numbers and codes. Every distinct number, code and identifier is a term of the index, which is why the index has 285.6 million unique terms. The text of images was not recognized, because [OCR]({{< ref "search/net/developer-guide/advanced-usage/indexing/ocr-support.md" >}}) was not used.

The text and the other data of the documents were extracted beforehand with the [Extractor](https://reference.groupdocs.com/search/net/groupdocs.search/extractor/) class and saved to files, as described in [Separate data extraction]({{< ref "search/net/developer-guide/advanced-usage/indexing/separate-data-extraction.md" >}}). The times in this article therefore do not include the extraction, which depends mostly on the formats of the documents.

| | Documents |
| --- | ---: |
| Extracted data files | 983,840 |
| Skipped: no text extracted | 63,943 |
| Rejected by the index as too large | 1 |
| Indexed | 919,896 |

The skipped files are mostly formats whose text was not extracted, such as PostScript, gzip, EPS, Flash and dBase files, and files that could not be read.

## Indexing

The extracted data was added to a new index in 182 calls of the [Add](https://reference.groupdocs.com/search/net/groupdocs.search/index/add/) method that takes an array of [ExtractedData](https://reference.groupdocs.com/search/net/groupdocs.search.common/extracteddata/) objects, about 5,000 documents per call on average:

| Operation | Time |
| --- | ---: |
| Indexing of 919,896 documents, in total | 3 h 48 min 33 s |
| - of which reading and deserializing the data files | 28 min 27 s |
| - of which the calls of the Add method | 3 h 19 min 57 s |

The index took 60.2 GB in 727 segments after the indexing.

Each call of the Add method took longer as the index grew, whatever the number of documents in the call:

| Calls | Documents in the index before the calls | Average time of a call | Average documents per call |
| --- | ---: | ---: | ---: |
| 1-10 | 0 | 12.7 s | 2,732 |
| 11-30 | 27,324 | 20.7 s | 2,858 |
| 31-60 | 84,485 | 33.3 s | 4,103 |
| 61-90 | 207,583 | 52.7 s | 4,338 |
| 91-120 | 337,721 | 80.1 s | 5,302 |
| 121-150 | 496,775 | 98.9 s | 4,730 |
| 151-182 | 638,677 | 109.4 s | 8,788 |

The last call added 1,240 documents in 112.7 s, as long as the calls before it with more than 9,000 documents each. In a large index, the number of calls, not the number of documents, therefore determines most of the indexing time. Add documents to a large index in batches as large as the memory allows, because the data of a whole batch is held in memory during the call.

The following example adds extracted data to an index in batches limited by the total size of the data files.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string extractedDataFolder = @"c:\MyExtractedData\";
long maxBatchLength = 512L * 1024 * 1024; // The total size of the data files of one batch

// Creating an index
Index index = new Index(indexFolder);

IndexingOptions options = new IndexingOptions();
options.Threads = 4; // From 1 to 4 indexing threads

List<ExtractedData> batch = new List<ExtractedData>();
long batchLength = 0;
foreach (string path in Directory.EnumerateFiles(extractedDataFolder, "*.dat", SearchOption.AllDirectories))
{
    // Reading the data extracted from a document
    byte[] bytes = File.ReadAllBytes(path);
    ExtractedData data = ExtractedData.Deserialize(bytes);
    if (data.IsExtractionSuccessful)
    {
        batch.Add(data);
        batchLength += bytes.Length;
    }

    if (batchLength >= maxBatchLength)
    {
        index.Add(batch.ToArray(), options); // Indexing a whole batch in one call
        batch.Clear();
        batchLength = 0;
    }
}

if (batch.Count > 0)
{
    index.Add(batch.ToArray(), options); // Indexing the rest
}

Console.WriteLine("Segments: " + index.IndexInfo.SegmentCount);
```

## Optimization

| Operation | Time | Result |
| --- | ---: | --- |
| Opening the index | 4.0 s | 727 segments |
| [Optimize](https://reference.groupdocs.com/search/net/groupdocs.search/index/optimize/) | 12 h 24 min 22 s | 28 segments; 60.2 GB -> 49.5 GB |

The [optimization]({{< ref "search/net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}) merges the segments of an index and physically removes deleted documents. The size of a merged segment is limited, so a large index is optimized into several segments, not into one. Searches are not blocked while an index is being optimized, so the optimization of a large index can run in the background, for example at night. All search times below were measured on the optimized index.

## Opening the index

An application opens an index by creating an [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) object. The optimized index of 919,896 documents opened in 9.8 s. Open an index once when the application starts and use the same object for all searches: a search does not change the index, and several searches can run on one object at the same time (see [Search thread safety]({{< ref "search/net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}})).

## Search

Every query ran 3 times in a row, and the garbage was collected before every run. The first run is what the first user of a just started application waits for. The repeated runs show the usual case of a running service, whose index files are already warmed up; wildcard, regular expression, numeric range and fuzzy searches also reuse the words matched by an earlier run.

| Kind of search | Query | Documents | Occurrences | First run | Repeated runs |
| --- | --- | ---: | ---: | ---: | ---: |
| Simple | `government` | 202,079 | 2,296,822 | 539 ms | 210 ms |
| [Boolean]({{< ref "search/net/developer-guide/advanced-usage/searching/boolean-search.md" >}}) | `(budget OR finance) AND report AND NOT draft` | 44,626 | 1,000,960 | 251 ms | 149 ms |
| [Phrase]({{< ref "search/net/developer-guide/advanced-usage/searching/phrase-search.md" >}}) | `"united states"` | 241,277 | 3,837,085 | 591 ms | 539 ms |
| [Phrase with wildcards]({{< ref "search/net/developer-guide/advanced-usage/searching/phrase-search.md" >}}) | `"department *0~~2 agriculture"` | 32,270 | 183,760 | 183 ms | 129 ms |
| [Wildcard]({{< ref "search/net/developer-guide/advanced-usage/searching/wildcard-search.md" >}}) | `manag?(1~6)` | 267,231 | 4,729,496 | 347 ms | 287 ms |
| [Regular expression]({{< ref "search/net/developer-guide/advanced-usage/searching/regular-expression-search.md" >}}) | `^^(.)\1{2,}` | 609,547 | 310,366,521 | 27.9 s | 26.5 s |
| [Fuzzy]({{< ref "search/net/developer-guide/advanced-usage/searching/fuzzy-search.md" >}}), similarity 0.8 | `goverment` | 202,183 | 2,298,177 | 207 ms | 157 ms |
| [Case-sensitive]({{< ref "search/net/developer-guide/advanced-usage/searching/case-sensitive-search.md" >}}) | `NASA` | 47,010 | 595,042 | 52 ms | 35 ms |
| [Faceted]({{< ref "search/net/developer-guide/advanced-usage/searching/faceted-search.md" >}}) | `content: energy` | 151,413 | 2,389,572 | 148 ms | 124 ms |
| [Numeric range]({{< ref "search/net/developer-guide/advanced-usage/searching/numeric-range-search.md" >}}) | `1000 ~~ 2000` | 703,582 | 230,532,220 | 25.2 s | 8.5 s |
| [Date range]({{< ref "search/net/developer-guide/advanced-usage/searching/date-range-search.md" >}}) | `daterange(2005-01-01 ~~ 2005-12-31)` | 138,876 | 4,554,549 | 92.1 s | 87.2 s |
| [Word forms]({{< ref "search/net/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}) | `managed` | 150,522 | 966,349 | 163 ms | 140 ms |
| [Synonyms]({{< ref "search/net/developer-guide/advanced-usage/searching/synonym-search.md" >}}) | `improve` | 202,299 | 1,265,491 | 211 ms | 168 ms |
| [Homophones]({{< ref "search/net/developer-guide/advanced-usage/searching/homophone-search.md" >}}) | `right` | 206,431 | 1,847,199 | 208 ms | 160 ms |
| [Spelling correction]({{< ref "search/net/developer-guide/advanced-usage/searching/spell-checking.md" >}}), 1 mistake | `managment` | 213,795 | 3,466,897 | 168 ms | 174 ms |
| [Keyboard layout correction]({{< ref "search/net/developer-guide/advanced-usage/searching/keyboard-layout-correction.md" >}}) | `ьфтфпуьуте` | 213,720 | 3,463,612 | 182 ms | 168 ms |
| [Aliases]({{< ref "search/net/developer-guide/advanced-usage/searching/using-aliases.md" >}}) | `@env AND water` | 78,118 | 3,457,853 | 203 ms | 143 ms |
| [Document filter]({{< ref "search/net/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}), PDF only | `government` | 88,732 | 1,083,176 | 147 ms | 131 ms |
| [By chunks]({{< ref "search/net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}), all 28 chunks | `government` | 202,079 | 2,296,822 | 155 ms | 137 ms |
| [Object query]({{< ref "search/net/developer-guide/advanced-usage/searching/queries-in-text-and-object-form.md" >}}) | `"enviromental *0~~2 protect?(0~4)"` | 37,935 | 215,993 | 456 ms | 236 ms |

The repeated runs column shows the average of the two repeated runs. The alias `env` stands for `(environment OR ecology OR pollution)`; the keyboard layout query is the word "management" typed in the Russian layout; the object query is a phrase of a fuzzy word, 0 to 2 arbitrary words and a word of "protect" and up to 4 more characters.

What the results show:

*   Most queries take tenths of a second although they are found in tens or hundreds of thousands of documents. The simple search ran first after the index was opened, so its first run also includes the warm-up of the process and of the index files.
*   The time of a search grows with its result. The three slowest queries are the heaviest that a user can ask: the regular expression matches 310 million occurrences in two thirds of the documents, the numeric range 231 million occurrences in three quarters of them, and the date range covers a whole year. Their repeated runs reuse the words matched by the first run, so the repeated times show that most of the time goes to collecting the occurrences: 26.5 s of 27.9 s for the regular expression and 8.5 s of 25.2 s for the numeric range.
*   Fuzzy search, spelling correction and keyboard layout correction took about as long as the simple search, which found a similar number of documents, although the index has 285.6 million terms.

## What the results depend on

### Documents

The number of documents is only one of the factors. The index size and the indexing time depend on the amount of text and on the number of distinct terms: data files full of numbers and codes produce far more terms than ordinary text. A scanned document contributes no text without [OCR]({{< ref "search/net/developer-guide/advanced-usage/indexing/ocr-support.md" >}}), and the [stored text]({{< ref "search/net/developer-guide/advanced-usage/indexing/storing-text-of-indexed-documents.md" >}}) of the documents, which highlighting and the retrieval of the text need, makes the index larger.

### Number of segments

Each call of the Add or [Update](https://reference.groupdocs.com/search/net/groupdocs.search/index/update/) method that indexes documents adds at least one segment to the index. A search reads the segments that contain its words, so the same documents in fewer segments are searched faster. Index in large batches and [optimize]({{< ref "search/net/developer-guide/advanced-usage/indexing/optimize-index.md" >}}) an index to which documents are often added. The [IndexInfo.SegmentCount](https://reference.groupdocs.com/search/net/groupdocs.search.common/indexinfo/segmentcount/) property returns the number of segments.

### Kind of query and size of the result

A query for a word reads the occurrences of that word only. A wildcard, regular expression, fuzzy or range query first finds all the words of the index that match it and then reads their occurrences, so its time depends on the number of words in the index and on the number of matching words. A wildcard or a regular expression that starts with a variable part has to check every word of the index. When an application does not need the whole result, limit it, for example with [document filters]({{< ref "search/net/developer-guide/advanced-usage/searching/document-filtering-in-search-result.md" >}}) or with the occurrence limits of the [search options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md" >}}), or narrow the query: a shorter range or a more specific pattern.

### Search threads

The [SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/) setting determines how many threads the searches of an index use together; the default is as many threads as there are processor cores. A single search uses them for its parallel parts: fuzzy search, wildcards, regular expressions, numeric ranges, spelling correction and image search. When many searches run at the same time, each of them runs on its own thread. For details, see [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md" >}}).

### Memory and disk

An index on disk keeps a part of its data in memory and reads the rest from the disk during a search. The operating system keeps the frequently read parts of the index files in its file cache, so the time of the repeated searches depends on the free memory of the computer. In the run described here, the working set of the benchmark process peaked at 14.4 GB, and the computer had 32 GB of memory. An index loaded with the [LoadIntoMemoryCompletely](https://reference.groupdocs.com/search/net/groupdocs.search/index/loadintomemorycompletely/) method does not read the disk, but needs about as much memory as the index takes on disk; see [Search index location]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-location.md" >}}).

## Measuring the performance on your documents

The numbers of this article were measured by the [GroupDocs.Search benchmarks](https://github.com/groupdocs-search/full-text-search-benchmark-net) application. It measures the extraction, the indexing, the optimization, 20 kinds of search and the reverse image search, reports the time and the peak memory of every operation and writes them to a CSV file. To measure your own documents:

1.  Clone the repository and install the .NET 10 SDK.
2.  In the configuration section of the `Program.cs` file, set the license file, the folders of the documents, of the extracted data, of the index and of the reports, and replace the queries with words that the users of your application search for.
3.  Run the application in the Release configuration: `dotnet run -c Release --project src/GroupDocs.Search.Benchmark`.

Any stage can be skipped: for example, extract the documents once and then repeat the indexing with other settings, or measure the search of an existing index only. Follow these rules to get comparable numbers:

*   Use documents that resemble the documents of your application, and record their number, formats and total size.
*   Apply a license, and run a Release build without a debugger.
*   Keep the index on a local disk, and make sure that no other process loads the processor or the disk. Antivirus software that scans files on access can slow down the indexing and the search.
*   Record the processor, the memory, the disk, the operating system and the versions of .NET and GroupDocs.Search together with the results.
*   Compare indexes with comparable numbers of segments, and compare versions of the library on the same documents and the same computer.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)

*   [GroupDocs.Search for .NET benchmarks](https://github.com/groupdocs-search/full-text-search-benchmark-net)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
