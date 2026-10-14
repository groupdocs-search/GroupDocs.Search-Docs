---
id: nesting-search-queries-in-object-form
url: search/net/nesting-search-queries-in-object-form
title: Nesting search queries in object form
weight: 11
description: "This article describes which search queries in object form can be nested in each other."
keywords: Nesting search queries in object form
productName: GroupDocs.Search for .NET
hideChildren: False
---
The table below shows all combinations of nesting search queries in object form with an indication of their possibility (+) or impossibility (–).

<table>
    <thead>
        <tr>
            <th>Nested query type</th>
            <th colspan=5>Parent query type</th>
        </tr>
        <tr>
            <th width="20%"></th>
            <th width="16%">Field</th>
            <th width="16%">PhraseSearch</th>
            <th width="16%">Not</th>
            <th width="16%">Or</th>
            <th width="16%">And</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Word</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>WordPattern</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>NumericRange</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Regex</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Field</td>
            <td>+</td>
            <td>–</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>DateRange</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>PhraseSearch</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Not</td>
            <td>+</td>
            <td>–</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Or</td>
            <td>+</td>
            <td>–</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>And</td>
            <td>+</td>
            <td>–</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>Wildcard</td>
            <td>–</td>
            <td>+</td>
            <td>–</td>
            <td>–</td>
            <td>–</td>
        </tr>
    </tbody>
</table>

An attempt to create an impossible combination throws ArgumentException with the message "Query of type '...' can not be a child of '...'.". A wildcard query created with the [CreateWildcardQuery](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/createwildcardquery/) method can be used only as a child of a phrase search query; passing it to the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method directly throws ArgumentException.

The [CreatePhraseSearchQuery](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/createphrasesearchquery/), [CreateAndQuery](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/createandquery/) and [CreateOrQuery](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/createorquery/) methods take an array of child queries (params), which must contain at least two queries. The [CreateAndQuery](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/createandquery/) and [CreateOrQuery](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/createorquery/) methods also have overloads with two child queries.

A created query can be examined with the following members of the [SearchQuery](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/) class:

*   The [ChildCount](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/childcount/) property returns the number of child queries.
*   The [GetChild](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/getchild/) method returns a child query by its index; the [FirstChild](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/firstchild/) and [SecondChild](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/secondchild/) properties return the first and the second child queries.
*   The [FieldName](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/fieldname/) property returns the name of the field to search in, specified by a field query that contains this query, or null.
*   The [SearchOptions](https://reference.groupdocs.com/search/net/groupdocs.search/searchquery/searchoptions/) property gets or sets the search options of this query.
*   The ToString method returns the query in text form.

The following example demonstrates the creation of a nested query and the traversal of the query tree.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";

// Creating an index in the specified folder
Index index = new Index(indexFolder);

// Indexing documents from the specified folder
index.Add(documentsFolder);

// Creating a query that searches for the phrase 'theory of * relativity' in the content field
SearchQuery phraseQuery = SearchQuery.CreatePhraseSearchQuery(
    SearchQuery.CreateWordQuery("theory"),
    SearchQuery.CreateWordQuery("of"),
    SearchQuery.CreateWildcardQuery(0, 1),
    SearchQuery.CreateWordQuery("relativity"));
SearchQuery fieldQuery = SearchQuery.CreateFieldQuery(CommonFieldNames.Content, phraseQuery);

// Printing the query tree
Console.WriteLine(fieldQuery.ToString());
for (int i = 0; i < phraseQuery.ChildCount; i++)
{
    SearchQuery child = phraseQuery.GetChild(i);
    Console.WriteLine("\t" + child.ToString() + " (field: " + child.FieldName + ")");
}

// Searching in the index
SearchResult result = index.Search(fieldQuery);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
