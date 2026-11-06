---
id: query-language-specification
url: search/net/query-language-specification
title: Query language specification
weight: 16
description: "This article presents the complete specification of the search query DSL used in text queries."
keywords: search query DSL, Domain Specific Language, Query language specification
productName: GroupDocs.Search for .NET
hideChildren: False
---
The text below represents the complete specification of the search query DSL (Domain Specific Language) used in text queries. See also [Search operation table]({{< ref "search/net/developer-guide/advanced-usage/searching/search-operation-table.md" >}}), [Search flow]({{< ref "search/net/developer-guide/advanced-usage/searching/search-flow.md" >}}).

Before a query is parsed, each alias reference in the query is replaced with the text associated with the alias in the alias dictionary of the index. An alias reference has the following syntax:

*alias-reference*:

*   @*alias-name*

*alias-name*:

*   One or more characters from the ranges a-z and 0-9, in any letter case (since v.26.11)

An alias reference is replaced only if it is preceded by the beginning of the query, a space or an opening parenthesis, and is followed by a space, a closing parenthesis or the end of the query. Since version 26.11, the text of an alias is inserted into the query as it is: an alias reference in the text of another alias is not replaced. For details, see the [Using aliases]({{< ref "search/net/developer-guide/advanced-usage/searching/using-aliases.md" >}}) page.

The syntax of a query after alias substitution is as follows.

*query*:

*   *regex-query*
*   *non-regex-query*

*regex-query*:

*   ^*pattern*

*non-regex-query*:

*   *unary-query*
*   *binary-query*

*unary-query*:

*   *word*
*   *word-pattern* (since v.18.12)
*   *phrase-query*
*   *field-name-query*
*   *numeric-range-query*
*   *date-range-query*
*   *parenthesized-query*
*   *not-query*

*phrase-query*:

*   " *phrase-item-list* "

*phrase-item-list*:

*   *phrase-item phrase-item*
*   *phrase-item-list phrase-item*

*phrase-item*:

*   *word*
*   *word-pattern* (since v.18.12)
*   *wildcard* (since v.18.1)

*word*:

*   any word without special characters

*word-pattern* (since v.18.12)

*   any non-special characters
*   ? (question marks)
*   ?(n\~m) (Wildcard groups, where n is *byte-number*, m is *byte-number*, and n <= m. For example: ?(1~2) )
*   *escape-sequence* (an escaped character of the pattern, not a wildcard; since v.26.11)

*wildcard*: (since v.18.1)

*   \* *byte-number*
*   \* *byte-number* ~~ *byte-number*

*byte-number*: (since v.18.1)

*   Any integer number in the range from 0 to 255, written with the digits 0-9

*field-name-query*:

*   *field-name*: *unary-query*

*numeric-range-query*:

*   *number* ~~ *number*

*number*:

*   Any integer number; a negative number is written with the hyphen-minus, for example -5. Since v.26.11, the numbers of a query are read in the same way in any culture of the current thread.

*date-range-query*:

*   daterange( *date* ~~ *date* )

*date*:

*   A date in the format \'yyyy-MM-dd\'. For example: 2019-09-16

*parenthesized-query*:

*   ( *non-regex-query* )

*not-query*:

*   ! *unary-query*
*   NOT *unary-query*

*binary-query*:

*   *and-query*
*   *or-query*

*and-query*:

*   *non-regex-query* & *unary-query*
*   *non-regex-query* AND *unary-query*

*or-query*:

*   *non-regex-query* | *unary-query*
*   *non-regex-query* || *unary-query*
*   *non-regex-query* OR *unary-query*

*escape-sequence:* (since v.19.2)

*   \\*N*  
    where *N* is one of  
    *( ) : \" & | ! ^ ~ \* ? \\*  
    or  
    *s* (for space character)
*   \\u*hhhh*  
    where *h* is hexadecimal digit (for any Unicode character)

Since version 26.11, a query can nest other queries, for example NOT queries and field queries, up to 128 levels deep. Parentheses and the AND and OR queries nested in one another are not counted, so a chain of any number of AND and OR operators can be searched. The search of a deeper query finds nothing, and the error is reported by the [ErrorOccurred](https://reference.groupdocs.com/search/net/groupdocs.search.events/eventhub/erroroccurred/) event of the index and in the warnings of the result.

Also since version 26.11, the following queries are parsed correctly: a word pattern followed by a closing parenthesis or a white space, for example `(te?t)` or `x AND (y OR te?(1~2)t)`; a word longer than 800 characters, for example a long regular expression; a word pattern with an escape sequence, for example `wh\?t?`; a numeric range with a negative number, for example `-5~~10`, in any culture of the current thread.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
