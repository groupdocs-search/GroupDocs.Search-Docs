---
id: managing-dictionaries-in-network
url: search/net/managing-dictionaries-in-network
title: Managing dictionaries in network
weight: 140
description: "This page contains information about managing dictionaries of shards in the search network."
keywords: managing dictionaries in search network, managing dictionaries in distributed index, working with dictionaries in search network, working with dictionaries in distributed index
productName: GroupDocs.Search for .NET
hideChildren: False
---
To obtain a dictionary from a shard of a search network node, use the [GetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getdictionary/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class. The first parameter of the [GetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getdictionary/) method is the type of the required dictionary, and the second parameter is the index of the shard in the network. The method returns an object of type [DictionaryBase](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/), which must be cast to the required dictionary type. In the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class there are also methods that immediately return the desired dictionary type without casting the type and without specifying the type in the parameters.

You can then perform all operations on the dictionary object that are valid for a dictionary in a single index. For information about working with dictionaries, see articles [Managing Dictionaries](https://docs.groupdocs.com/search/net/managing-dictionaries/).

And after setting up the dictionary, you need to return the dictionary object back to the shard using the [SetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/setdictionary/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class, specifying the index of the desired shard. Alternatively, you can set the dictionary object to all shards by the [SetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/setdictionary/) method with one parameter without specifying the shard index.

The following code example demonstrates working with dictionaries on the search network.

**C#**

```csharp
Console.WriteLine("Adding synonyms");

Indexer indexer = node.Indexer;

int[] indices = node.GetShardIndices();
SynonymDictionary dictionary = indexer.GetSynonymDictionary(indices[0]);

if (clearBeforeAdding)
{
    dictionary.Clear();
}
dictionary.AddRange(new string[][] { group });

indexer.SetDictionary(dictionary);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
