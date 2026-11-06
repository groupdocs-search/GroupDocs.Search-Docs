---
id: wildcard-search
url: search/net/wildcard-search
title: Wildcard search
weight: 29
description: "This article describes wildcard search, which allows you to search for words with unknown letters or ranges of letters."
keywords: Wildcard search
productName: GroupDocs.Search for .NET
hideChildren: False
---
Wildcard search allows you to search for words with unknown letters or ranges of letters.

In a search query in text form, there are 2 forms of wildcard characters:

*   ? for a single character;
*   ?(*n*~*m*) for a group of characters, where *n* and *m* are numbers from 0 to 255, and *n* <= *m*.

Wildcard search is similar to [regular expression search]({{< ref "search/net/developer-guide/advanced-usage/searching/regular-expression-search.md" >}}), but it works significantly faster when there are fewer groups of wildcard characters and they are closer to the end of a search query.

Wildcard search is flexible enough to be used for prefix queries, since a prefix query is a special case of a wildcard query. Since version 26.10, wildcard search uses as many threads as there are processor cores by default. Since version 26.11, the searches of an index that run at the same time share these threads (see the [Search thread safety]({{< ref "search/net/developer-guide/advanced-usage/searching/search-thread-safety.md" >}}) page).

A wildcard counts the characters of a word as UTF-16 code units: a character above the basic multilingual plane, for example an emoji that the alphabet of the index makes a letter, counts as two characters, which two one-character wildcards or a wildcard of two characters match.

Examples of wildcard search with queries in text form are presented below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Search in the index
SearchResult result1 = index.Search("m?rry"); // Search for 'merry', 'marry', etc.
SearchResult result2 = index.Search("card?(1~6)"); // Search for 'cardiff', 'cardinal', 'cardio', 'cards', etc.
```

To build a query for the wildcard search in object form, use the [WordPattern](https://reference.groupdocs.com/search/net/groupdocs.search.common/wordpattern/) class. This class contains the following methods for adding known parts of a word and wildcards to a pattern:

*   [AppendString](https://reference.groupdocs.com/search/net/groupdocs.search.common/wordpattern/appendstring/) appends a string of known characters.
*   [AppendCharacter](https://reference.groupdocs.com/search/net/groupdocs.search.common/wordpattern/appendcharacter/) appends a known character.
*   [AppendOneCharacterWildcard](https://reference.groupdocs.com/search/net/groupdocs.search.common/wordpattern/appendonecharacterwildcard/) appends a wildcard for exactly one character (? in text form).
*   [AppendZeroOrOneCharacterWildcard](https://reference.groupdocs.com/search/net/groupdocs.search.common/wordpattern/appendzerooronecharacterwildcard/) appends a wildcard for zero or one character (?(0~1) in text form).
*   [AppendZeroOrMoreCharactersWildcard](https://reference.groupdocs.com/search/net/groupdocs.search.common/wordpattern/appendzeroormorecharacterswildcard/) appends a wildcard for zero to 255 characters (?(0~255) in text form).
*   [AppendOneOrMoreCharactersWildcard](https://reference.groupdocs.com/search/net/groupdocs.search.common/wordpattern/appendoneormorecharacterswildcard/) appends a wildcard for one to 255 characters (?(1~255) in text form).
*   [AppendWildcard](https://reference.groupdocs.com/search/net/groupdocs.search.common/wordpattern/appendwildcard/) appends a wildcard for the specified minimum and maximum numbers of characters (?(min~max) in text form). Both values must be in the range from 0 to 255, and the minimum must not be greater than the maximum; otherwise, ArgumentOutOfRangeException is thrown.

The known characters are converted to lower case, since words are stored in the index in lower case. An example of constructing a query in object form is presented below.

**C#**

```csharp
string indexFolder = @"c:\MyIndex\";
string documentsFolder = @"c:\MyDocuments\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.Add(documentsFolder);
 
// Search with pattern "m?rry"
// Search for 'merry', 'marry', etc.
WordPattern pattern1 = new WordPattern();
pattern1.AppendString("m");
pattern1.AppendOneCharacterWildcard();
pattern1.AppendString("rry");
SearchQuery query1 = SearchQuery.CreateWordPatternQuery(pattern1);
SearchResult result1 = index.Search(query1);
 
// Search with pattern "card?(1~6)"
// Search for 'cardiff', 'cardinal', 'cardio', 'cards', etc.
WordPattern pattern2 = new WordPattern();
pattern2.AppendString("card");
pattern2.AppendWildcard(1, 6);
SearchQuery query2 = SearchQuery.CreateWordPatternQuery(pattern2);
SearchResult result2 = index.Search(query2);
```

Since version 26.11, the following rules apply to wildcard search:

*   In a query in text form, a backslash escapes a character of a word pattern, as in a word: an escaped character is a character of the pattern, not a wildcard. For example, in an index whose alphabet makes the question mark a letter, the pattern `wh\?t?` finds "wh?ts" and not "whats". A backslash that is not followed by a character that can be escaped makes the query invalid. See the [Search for text containing special characters]({{< ref "search/net/developer-guide/advanced-usage/searching/search-for-text-containing-special-characters.md" >}}) page.
*   A pattern whose wildcards need more characters than the longest word that an index holds (80 characters), for example `?(100~200)`, finds nothing. Before, such a pattern found all the words of 80 characters.
*   A pattern that is too complex to be searched for, whose wildcards of a limited length allow more than about 4000 characters in total, finds nothing, and the error is reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index and in the warnings of the result. Before, the [Search](https://reference.groupdocs.com/search/net/groupdocs.search/index/search/) method threw ArgumentException.
*   A search is canceled (see the [Search options]({{< ref "search/net/developer-guide/advanced-usage/searching/search-options.md" >}}) page) also while it looks for the words of the index that match the pattern.
*   The words that match a pattern are kept for the next searches with the same query until the index is changed, so a repeated search is faster.
*   In a case-sensitive search, a word pattern finds the words in any letter case, and the warnings of the result tell it (see the [Case sensitive search]({{< ref "search/net/developer-guide/advanced-usage/searching/case-sensitive-search.md" >}}) page).

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
