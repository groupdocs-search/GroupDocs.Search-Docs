---
id: search-results
url: search/net/search-results
title: Search results
weight: 24
description: "This article describes the classes that represent search results and shows how to work with them."
keywords: Search results
productName: GroupDocs.Search for .NET
hideChildren: False
---
Search results are represented by the [SearchResult](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/) class, an instance of which is returned by the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method of the [Index](https://reference.groupdocs.com/search/net/groupdocs.search/index/) class. The [Search](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/search/) method of the [IndexRepository](https://reference.groupdocs.com/search/net/groupdocs.search/indexrepository/) class also returns an instance of the [SearchResult](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/) class.

The [SearchResult](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/) class contains the following members:

*   The [DocumentCount](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/documentcount/) property returns the number of documents found.
*   The [OccurrenceCount](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/occurrencecount/) property returns the total number of occurrences found.
*   The [Truncated](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/truncated/) property returns a value indicating that the result is truncated due to limits specified in the search options.
*   The [Warnings](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/warnings/) property returns warnings describing the result, for example, a warning about the presence of a stop word in a search query.
*   The [NextChunkSearchToken](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/nextchunksearchtoken/) property returns a chunk search token to search for the next chunk, or null if there are no more chunks. For details on search by chunks, see the [Search by chunks]({{< ref "search/net/developer-guide/advanced-usage/searching/search-by-chunks.md" >}}) page.
*   The [StartTime](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/starttime/) property returns the start time of the search.
*   The [EndTime](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/endtime/) property returns the end time of the search.
*   The [SearchDuration](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/searchduration/) property returns the search duration.
*   The [GetFoundDocument](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/getfounddocument/) method returns the found document by index.
*   The [GetEnumerator](https://reference.groupdocs.com/search/net/groupdocs.search.results/searchresult/getenumerator/) method returns an enumerator that iterates through the collection of the documents found.

The found document is represented by an instance of a [FoundDocument](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/) class. The [FoundDocument](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/) class contains the following members:

*   The [DocumentInfo](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/documentinfo/) property returns the document info object. Its members are described below.
*   The [Relevance](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/relevance/) property returns the relevance of the document in the search result.
*   The [OccurrenceCount](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/occurrencecount/) property returns the number of occurrences found in the document.
*   The [FoundFields](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/foundfields/) property returns the found document fields.
*   The [Terms](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/terms/) property returns the found terms. The value is evaluated each time the property is accessed based on the data for each document field found.
*   The [TermSequences](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/termsequences/) property returns the found term sequences.
*   The [Attributes](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/attributes/) property returns the attributes associated with the found document. The attributes are retrieved only if the [RetrieveAttributes](https://reference.groupdocs.com/search/net/groupdocs.search.options/searchoptions/retrieveattributes/) search option is enabled; otherwise, the property returns an empty array. For details, see the [Search options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md" >}}) page.
*   The [Serialize](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/serialize/) method serializes the current found document instance to a byte array.
*   The [Deserialize](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocument/deserialize/) method deserializes an instance of found document from a byte array.

The indexed document is described by an instance of the [DocumentInfo](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/) class, which contains the following members:

*   The [FilePath](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/filepath/) property returns the file path for a document indexed from a file, or the document key for a document indexed from a stream or a structure.
*   The [FileType](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/filetype/) property returns the file type of the document.
*   The [FormatFamily](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/formatfamily/) property returns the format family of the document.
*   The [InnerPath](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/innerpath/) property returns the inner path of an item of a container document, for example, an item of a ZIP archive or an attachment of an email message.
*   The [InnerPathParts](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/innerpathparts/) property returns the parts of the inner path of an item of a container document.
*   The [DocumentSourceKind](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/documentsourcekind/) property returns the kind of the document source: a file, a stream, a structure, previously extracted data, or a temporary file.
*   The [ModificationTime](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/modificationtime/) property returns the modification time of the document at the time of indexing; for a document indexed from a file, this is the last write time of the file.
*   The [IndexedWithError](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/indexedwitherror/) property returns a value indicating whether an error occurred while indexing the document, for example, if the indexing of the document was interrupted by cancellation. Such a document indexed from a file is indexed again by the next [Update](https://reference.groupdocs.com/search/net/groupdocs.search/index/update/) operation; a document indexed from a stream, a structure or extracted data has to be added again.
*   The [Serialize](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/serialize/) and [Deserialize](https://reference.groupdocs.com/search/net/groupdocs.search.results/documentinfo/deserialize/) methods serialize the document info to a byte array and deserialize it from a byte array.

The found document field is represented by an instance of a [FoundDocumentField](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/) class. The [FoundDocumentField](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/) class contains the following members:

*   The [FieldName](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/fieldname/) property returns the field name.
*   The [OccurrenceCount](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/occurrencecount/) property returns the number of occurrences found.
*   The [Terms](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/terms/) property returns the terms found.
*   The [TermsOccurrences](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/termsoccurrences/) property returns the occurrences of the found terms.
*   The [TermSequences](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/termsequences/) property returns the term sequences found.
*   The [TermSequencesOccurrences](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/termsequencesoccurrences/) property returns the occurrences of the found term sequences.
*   The [Serialize](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/serialize/) method serializes the current found document field instance to a byte array.
*   The [Deserialize](https://reference.groupdocs.com/search/net/groupdocs.search.results/founddocumentfield/deserialize/) method deserializes an instance of found document field from a byte array.

Since version 26.10, search uses as many threads as there are processor cores by default. Therefore, the order of the terms found by fuzzy, wildcard, regular expression, numeric range search and spelling correction may differ from run to run. To get the same order every time, set the [SearchThreads](https://reference.groupdocs.com/search/net/groupdocs.search/indexsettings/searchthreads/) index setting to NumberOfThreads.One. For details, see the [Search index settings]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/search-index-settings.md#searchthreads-property" >}}) page.

The following example shows how to print information on the documents found in the console.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentFolder = @"c:\MyDocuments\";
 
// Creating an index
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentFolder);
 
// Creating search options
SearchOptions options = new SearchOptions();
options.FuzzySearch.Enabled = true; // Enabling the fuzzy search
options.FuzzySearch.FuzzyAlgorithm = new TableDiscreteFunction(3); // Setting the maximum number of differences to 3
 
// Search for documents containing the word 'Einstein' or the phrase 'Theory of Relativity'
SearchResult result = index.Search("Einstein OR \"Theory of Relativity\"", options);
 
// Printing the result
Console.WriteLine("Documents: " + result.DocumentCount);
Console.WriteLine("Total occurrences: " + result.OccurrenceCount);
for (int i = 0; i < result.DocumentCount; i++)
{
    FoundDocument document = result.GetFoundDocument(i);
    Console.WriteLine("\tDocument: " + document.DocumentInfo.FilePath);
    Console.WriteLine("\tOccurrences: " + document.OccurrenceCount);
    for (int j = 0; j < document.FoundFields.Length; j++)
    {
        FoundDocumentField field = document.FoundFields[j];
        Console.WriteLine("\t\tField: " + field.FieldName);
        Console.WriteLine("\t\tOccurrences: " + field.OccurrenceCount);
        // Printing found terms
        if (field.Terms != null)
        {
            for (int k = 0; k < field.Terms.Length; k++)
            {
                Console.WriteLine("\t\t\t" + field.Terms[k].PadRight(20) + field.TermsOccurrences[k]);
            }
        }
        // Printing found phrases
        if (field.TermSequences != null)
        {
            for (int k = 0; k < field.TermSequences.Length; k++)
            {
                string sequence = string.Join(" ", field.TermSequences[k]);
                Console.WriteLine("\t\t\t" + sequence.PadRight(30) + field.TermSequencesOccurrences[k]);
            }
        }
    }
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
