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
To obtain a dictionary from a shard of the search network, use the [GetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getdictionary/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class. The first parameter of the [GetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getdictionary/) method is the type of the required dictionary, and the second parameter is the index of the shard in the network. The method returns an object of type [DictionaryBase](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionarybase/), which must be cast to the required dictionary type.

The [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class also has methods that return a dictionary of the required type without casting: [GetAlphabet](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getalphabet/), [GetTokenizationDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/gettokenizationdictionary/), [GetCharacterReplacementDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getcharacterreplacementdictionary/), [GetStopWordDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getstopworddictionary/), [GetSynonymDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getsynonymdictionary/), [GetHomophoneDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/gethomophonedictionary/), [GetAliasDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getaliasdictionary/), [GetSpellingCorrector](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getspellingcorrector/) and [GetPasswordDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/getpassworddictionary/).

The returned object is a copy of the dictionary of the shard. You can perform all operations on it that are valid for a dictionary in a single index; the changes do not affect the shard until the dictionary is set back. For information about working with dictionaries, see the [Managing dictionaries]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

After setting up the dictionary, return it to the shard using the [SetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/setdictionary/) method of the [Indexer](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/) class, specifying the index of the desired shard. Alternatively, you can set the dictionary to all shards by calling the [SetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/setdictionary/) method with one parameter, without specifying the shard index. The [SetDictionary](https://reference.groupdocs.com/search/net/groupdocs.search.scaling/indexer/setdictionary/) method replaces the whole content of the dictionary of a shard with the content of the passed dictionary.

{{< alert style="info" >}}The alphabet, the tokenization dictionary, the stop word dictionary and the character replacement dictionary define how the text of documents is divided into terms. Therefore, a shard accepts another content of such a dictionary only while its index contains no documents and is not performing an operation. A shard that does not accept the dictionary does not throw an exception: it keeps its own dictionary and reports the reason as an error to the log of the search network and by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.scaling.events/nodeeventhub/erroroccurred/) event of its node. Configure these dictionaries before adding the first documents to the search network. The other dictionaries can be changed at any time. See also the [Index format and compatibility]({{< ref "search/net/developer-guide/advanced-usage/creating-an-index/index-format-and-compatibility.md" >}}) page.{{< /alert >}}

The examples on this page use the master node of a running search network. The following code creates and starts the master node as described on the [Search network deployment]({{< ref "search/net/developer-guide/advanced-usage/scaling/search-network-deployment.md" >}}) page; the slave nodes must be started beforehand.

**C#**

```csharp
string basePath = @"c:\SearchNetwork\";
int basePort = 49100;

// Loading the search network configuration from a file
Configuration configuration = Configuration.Load(basePath + "Configuration.xml");

// Creating, configuring and starting the master node; the timeouts are specified in milliseconds
SearchNetworkNode node = SearchNetworkNode.CreateMasterNode(
    0,
    basePath + "Node0",
    new TcpSettings(basePort, 3000, 3000, 3000, 1000),
    new FileLogger(basePath + "Log.txt", 4.0),
    configuration);
node.ConfigureAllNodes();
node.Start();
```

The following code example, which continues the previous one, demonstrates adding a group of synonyms to the synonym dictionary of all shards.

**C#**

```csharp
string[] group = new string[] { "achieve", "accomplish", "attain", "reach" };
bool clearBeforeAdding = false; // Specifies whether to remove the existing synonyms

Console.WriteLine("Adding synonyms");

Indexer indexer = node.Indexer;

// Getting the synonym dictionary of the first shard
int[] indices = node.GetShardIndices();
SynonymDictionary dictionary = indexer.GetSynonymDictionary(indices[0]);

if (clearBeforeAdding)
{
    dictionary.Clear();
}
dictionary.AddRange(new string[][] { group });

// Setting the dictionary to all shards
indexer.SetDictionary(dictionary);
```

The following code example, which continues the code that creates the master node, demonstrates setting the tokenization dictionary of all shards before adding documents to the search network.

**C#**

```csharp
Indexer indexer = node.Indexer;

// Getting the tokenization dictionary of the first shard
int[] indices = node.GetShardIndices();
Alphabet tokenizationDictionary = indexer.GetTokenizationDictionary(indices[0]);

// Setting the Letter type for Hiragana and Katakana characters
List<int> codePoints = new List<int>();
for (int codePoint = 0x3040; codePoint <= 0x30FF; codePoint++)
{
    codePoints.Add(codePoint);
}
tokenizationDictionary.SetRange(codePoints.ToArray(), CharacterType.Letter);

// Setting the dictionary to all shards; a shard whose index contains documents keeps its own dictionary
indexer.SetDictionary(tokenizationDictionary);
```

For more information on the tokenization dictionary, see the [Tokenization dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary.md" >}}) page.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
