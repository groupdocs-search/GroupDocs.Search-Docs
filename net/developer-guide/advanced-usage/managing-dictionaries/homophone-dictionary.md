---
id: homophone-dictionary
url: search/net/homophone-dictionary
title: Homophone dictionary
weight: 5
description: "This article describes the API for managing the homophone dictionary of an index."
keywords: homophone dictionary, homophones
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [HomophoneDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/homophonedictionary/) class is designed to store homophones in an index. For information on searching using homophones, see the [Homophone search]({{< ref "search/net/developer-guide/advanced-usage/searching/homophone-search.md" >}}) page.

The homophone dictionary can be changed at any time, also in an index that contains documents. The dictionary keeps words in lower case without leading and trailing white space, so the methods that get homophones ignore the letter case of the specified word.

To get the number of words in the dictionary, use the [Count](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/homophonedictionary/count/) property.

To add groups of homophones to the dictionary, use the [AddRange](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/homophonedictionary/addrange/) method.

The [Clear](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/homophonedictionary/clear/) method is used to remove all homophones from the dictionary.

The [GetHomophones](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/homophonedictionary/gethomophones/) method is used to get a list of homophones for a given word. The returned list does not contain the word itself.

The [GetHomophoneGroups](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/homophonedictionary/gethomophonegroups/) method is used to get all homophone groups to which a given word belongs.

To get all homophone groups from the dictionary, use the [GetAllHomophoneGroups](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/homophonedictionary/getallhomophonegroups/) method.

To export homophones to a file, use the [ExportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/exportdictionary/) method.

To import homophones from a file, use the [ImportDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/importdictionary/) method. Errors of exporting and importing are reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index instead of exceptions.

The following example demonstrates the use of methods of the homophone dictionary.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";

// Creating or opening an index in the specified folder
Index index = new Index(indexFolder);

// Getting homophones for the word 'braid'; the letter case of the word is ignored
string[] homophones = index.Dictionaries.HomophoneDictionary.GetHomophones("Braid");
Console.WriteLine("Homophones for 'braid':");
for (int i = 0; i < homophones.Length; i++)
{
    Console.WriteLine(homophones[i]);
}

// Getting groups of homophones to which the word 'braid' belongs
string[][] groups = index.Dictionaries.HomophoneDictionary.GetHomophoneGroups("braid");
Console.WriteLine("Homophone groups for 'braid':");
for (int i = 0; i < groups.Length; i++)
{
    string[] group = groups[i];
    for (int j = 0; j < group.Length; j++)
    {
        Console.Write(group[j] + " ");
    }
    Console.WriteLine();
}

if (index.Dictionaries.HomophoneDictionary.Count > 0)
{
    // Removing all homophones from the dictionary
    index.Dictionaries.HomophoneDictionary.Clear();
}

// Adding homophones to the dictionary
string[][] homophoneGroups = new string[][]
{
    new string[] { "awe", "oar", "or", "ore" },
    new string[] { "aye", "eye", "i" },
};
index.Dictionaries.HomophoneDictionary.AddRange(homophoneGroups);

// Exporting homophones to a file
index.Dictionaries.HomophoneDictionary.ExportDictionary(@"c:\Homophones.dat");

// Importing homophones from a file
index.Dictionaries.HomophoneDictionary.ImportDictionary(@"c:\Homophones.dat");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
