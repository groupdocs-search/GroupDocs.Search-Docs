---
id: using-aliases
url: search/net/using-aliases
title: Using aliases
weight: 28
description: "This article describes the use of aliases, which allows you to reduce the length of search queries in text form."
keywords: Using aliases
productName: GroupDocs.Search for .NET
hideChildren: False
---
The use of aliases allows you to reduce the length of search queries in text form. To use an alias in a search query, you must enter the @ symbol with the name of the alias after it without spaces.

Before a search query in text form is parsed, each alias in the query is replaced with the text associated with it in the alias dictionary (see [Search flow]({{< ref "search/net/developer-guide/advanced-usage/searching/search-flow.md" >}})). The following rules apply:

*   An alias name can contain only characters from the ranges a-z and 0-9; an attempt to add an alias with other characters throws ArgumentException. Alias names are stored in lower case, so the name used in a query must be written in lower case.
*   An alias is replaced only if it is separated from the adjacent text by spaces or parentheses, or is located at the beginning or end of the query. For example, in the query "@t OR (@e)", both aliases are replaced.
*   Aliases are not used in search queries in object form.

It is recommended to enclose the text of an alias in parentheses if it contains operators, so that the substituted text is combined with the rest of the query as a single subquery, as in the example below.

Aliases are stored in the alias dictionary. By default, the alias dictionary is empty. The alias dictionary can be changed at any time. Detailed information on managing the alias dictionary can be found on the [Alias dictionary]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/alias-dictionary.md" >}}) page of the [Managing dictionaries]({{< ref "search/net/developer-guide/advanced-usage/managing-dictionaries/_index.md" >}}) section.

An example of using aliases for searching is presented below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Adding aliases to the alias dictionary
index.Dictionaries.AliasDictionary.Add("t", "(theory OR relativity)");
index.Dictionaries.AliasDictionary.Add("e", "(Einstein OR Albert)");
 
// Search in the index
SearchResult result = index.Search("@t OR @e");
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
