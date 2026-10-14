---
id: synonym-dictionary
url: search/net/synonym-dictionary
title: Synonym dictionary
weight: 8
description: "This article describes the API for managing the synonym dictionary of an index."
keywords: synonym dictionary, synonyms
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [SynonymDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/synonymdictionary/) class is designed to store synonyms in an index. For information on searching using synonyms, see the [Synonym search]({{< ref "search/net/developer-guide/advanced-usage/searching/synonym-search.md" >}}) page.

The synonym dictionary can be changed at any time, also in an index that contains documents. The dictionary keeps words in lower case without leading and trailing white space, so the methods that get synonyms ignore the letter case of the specified word.

To get the number of words in the dictionary, use the [Count](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/synonymdictionary/count/) property.

To add groups of synonyms to the dictionary, use the [AddRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/synonymdictionary/addrange/) method.

The [Clear](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/synonymdictionary/clear/) method is used to remove all synonyms from the dictionary.

The [GetSynonyms](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/synonymdictionary/getsynonyms/) method is used to get a list of synonyms for a given word. The returned list does not contain the word itself.

The [GetSynonymGroups](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/synonymdictionary/getsynonymgroups/) method is used to get all synonym groups to which a given word belongs.

To get all synonym groups from the dictionary, use the [GetAllSynonymGroups](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/synonymdictionary/getallsynonymgroups/) method.

To export synonyms to a file, use the [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) method.

To import synonyms from a file, use the [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) method. Errors of exporting and importing are reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the synonym dictionary.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Creating or opening an index in the specified folder
Index index = new Index(indexFolder);

// Getting synonyms for the word 'make'; the letter case of the word is ignored
string[] synonyms = index.Dictionaries.SynonymDictionary.GetSynonyms("Make");
Console.WriteLine("Synonyms for 'make':");
for (int i = 0; i < synonyms.Length; i++)
{
    Console.WriteLine(synonyms[i]);
}

// Getting groups of synonyms to which the word 'make' belongs
string[][] groups = index.Dictionaries.SynonymDictionary.GetSynonymGroups("make");
Console.WriteLine("Synonym groups for 'make':");
for (int i = 0; i < groups.Length; i++)
{
    string[] group = groups[i];
    for (int j = 0; j < group.Length; j++)
    {
        Console.Write(group[j] + " ");
    }
    Console.WriteLine();
}

if (index.Dictionaries.SynonymDictionary.Count > 0)
{
    // Removing all synonyms from the dictionary
    index.Dictionaries.SynonymDictionary.Clear();
}

// Adding synonyms to the dictionary
string[][] synonymGroups = new string[][]
{
    new string[] { "achieve", "accomplish", "attain", "reach" },
    new string[] { "accept", "take", "have" },
};
index.Dictionaries.SynonymDictionary.AddRange(synonymGroups);

// Exporting synonyms to a file
index.Dictionaries.SynonymDictionary.ExportDictionary(@"c:\Synonyms.dat");

// Importing synonyms from a file
index.Dictionaries.SynonymDictionary.ImportDictionary(@"c:\Synonyms.dat");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
