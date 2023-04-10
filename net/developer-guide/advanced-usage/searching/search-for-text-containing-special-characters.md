---
id: search-for-text-containing-special-characters
url: search/net/search-for-text-containing-special-characters
title: Search for text containing special characters
weight: 20.5
description: "This article shows how to handle search queries if they contain special characters and separator characters."
keywords: Search for text containing special characters
productName: GroupDocs.Search for .NET
hideChildren: False
---
In the case where the search query must contain special characters that are not control characters, these characters must be escaped.
The special characters are:
<pre>
( ) : " & | ! ^ ~ * ? \ <i>space</i>
</pre>
These characters can be escaped by adding a \\ sign to the left of the character. For example:  
\\& - escaping & character;  
\\~ - escaping ~ character;  
\\s - escaping space character.  
See [Search operation table]({{< ref "search/net/developer-guide/advanced-usage/searching/search-operation-table.md" >}}) for detailes on search query syntax.

However, it must be remembered that if the escaped character is a separator, then it will still not be found in the text, since it is not indexed. And as a result, words containing such characters will also not be found when searching for an exact match.

If the search query contains separator characters, words containing these characters will not be found. Since, in fact, when indexing, these characters break the word into parts, and they themselves are deleted and not indexed.
Therefore, when performing a search, the query must be checked for the content of separator characters. Each word containing such characters should be broken into several smaller words and form a search phrase.

The order of processing each word in a search query is as follows:
1. If separators are present in the word, they are replaced by spaces, and the word search is converted to a phrase search.
2. If special characters are present in the word, they are escaped.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Setting character types
index.Dictionaries.Alphabet.SetRange(new char[] { '&' }, CharacterType.Letter);
index.Dictionaries.Alphabet.SetRange(new char[] { '-' }, CharacterType.Separator);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Defining a search query
string word = "rock&roll-music";

// Replacing separators with the space characters
StringBuilder result = new StringBuilder();
for (int i = 0; i < word.Length; i++)
{
    char character = word[i];
    CharacterType characterType = index.Dictionaries.Alphabet.GetCharacterType(character);
    if (characterType == CharacterType.Separator)
    {
        result.Append(' ');
    }
    else
    {
        result.Append(character);
    }
}

// Escaping special characters
const string specialCharacters = "():\"&|!^~*?\\";
for (int i = result.Length - 1; i >= 0; i--)
{
    char c = result[i];
    if (specialCharacters.Contains(c.ToString()))
    {
        result.Insert(i, '\\');
    }
}

string query = result.ToString();
if (query.Contains(" "))
{
    query = "\"" + query + "\"";
}

// Searching
SearchResult searchResult = index.Search(query);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
