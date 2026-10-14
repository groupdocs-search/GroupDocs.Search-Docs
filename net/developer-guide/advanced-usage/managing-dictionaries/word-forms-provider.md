---
id: word-forms-provider
url: search/net/word-forms-provider
title: Word forms provider
weight: 10
description: "This article describes the IWordFormsProvider interface and how to set a custom provider of word forms for an index."
keywords: word forms provider, IWordFormsProvider, word forms
productName: GroupDocs.Search for .NET
hideChildren: False
---
The [IWordFormsProvider](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/iwordformsprovider/) interface is designed to implement a word forms provider for searching by word forms. For information on searching by word forms, see the [Search for different word forms]({{< ref "search/net/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}) page.

The [IWordFormsProvider](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/iwordformsprovider/) interface contains only one [GetWordForms](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/iwordformsprovider/getwordforms/) method, which returns various forms for the word passed as an argument. An example implementation of a simple provider of word forms is presented below.

**C#**

```csharp
public class SimpleWordFormsProvider : IWordFormsProvider
{
    public string[] GetWordForms(string word)
    {
        List<string> result = new List<string>();

        // We assume that the input word is in the plural, then we add the singular
        if (word.Length > 3 &&
            word.EndsWith("ies", StringComparison.InvariantCultureIgnoreCase))
        {
            result.Add(word.Substring(0, word.Length - 3) + "y");
        }
        if (word.Length > 2 &&
            word.EndsWith("es", StringComparison.InvariantCultureIgnoreCase))
        {
            result.Add(word.Substring(0, word.Length - 2));
        }
        if (word.Length > 1 &&
            word.EndsWith("s", StringComparison.InvariantCultureIgnoreCase))
        {
            result.Add(word.Substring(0, word.Length - 1));
        }

        // Then we assume that the input word is in the singular, we add the plural
        if (word.Length > 1 &&
            word.EndsWith("y", StringComparison.InvariantCultureIgnoreCase))
        {
            result.Add(word.Substring(0, word.Length - 1) + "ies");
        }
        else if (word.EndsWith("s", StringComparison.InvariantCultureIgnoreCase) ||
            word.EndsWith("x", StringComparison.InvariantCultureIgnoreCase) ||
            word.EndsWith("ch", StringComparison.InvariantCultureIgnoreCase) ||
            word.EndsWith("sh", StringComparison.InvariantCultureIgnoreCase))
        {
            result.Add(word + "es");
        }
        else
        {
            result.Add(word + "s");
        }
        // All rules are implemented in the EnglishWordFormsProvider class

        return result.ToArray();
    }
}
```

By default, the **[EnglishWordFormsProvider](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/englishwordformsprovider/)** class is used, which for English generates various forms of nouns, adjectives, pronouns, verbs, etc. A custom provider is set with the [WordFormsProvider](https://reference.groupdocs.com/search/net/groupdocs.search.dictionaries/dictionaryrepository/wordformsprovider/) property of the [Index.Dictionaries](https://reference.groupdocs.com/search/net/groupdocs.search/index/dictionaries/) object. The provider can be replaced at any time; it is not saved in the index, so it must be set each time the index is opened. An example of setting a custom provider of word forms is presented below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating or opening an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Setting the custom word forms provider instance
index.Dictionaries.WordFormsProvider = new SimpleWordFormsProvider();

// Creating a search options instance
SearchOptions options = new SearchOptions();
options.UseWordFormsSearch = true; // Enabling search for word forms

// Searching in the index
SearchResult result = index.Search("relative", options);

// The following words can be found:
// relative
// relatives
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
