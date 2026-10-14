---
id: search-operation-table
url: search/net/search-operation-table
title: Search operation table
weight: 21
description: "This article provides the syntax of all elements allowed in text search queries."
keywords: Search operation table
productName: GroupDocs.Search for .NET
hideChildren: False
---
The table below provides the syntax of all elements allowed in text search queries. Phrases in the examples are given as raw query text, without escaping for C# string literals. See also [Query language specification]({{< ref "search/net/developer-guide/advanced-usage/searching/query-language-specification.md" >}}), [Search flow]({{< ref "search/net/developer-guide/advanced-usage/searching/search-flow.md" >}}).

| Element | Syntax | Description | Examples |
| --- | --- | --- | --- |
| Parentheses | ( *inner-query* ) | Parentheses are used to specify order of operations. | <ul><li>term1 &#124; (term2 &amp; term3)</li><li>(\"total expenses\" &#124; \"total costs\") &amp; (82000 ~~ 83000)</li></ul> |
| Field specifier | *field-name* : *inner-query* | Field specifier is used to specify field name. | <ul><li>content : term</li><li>creationdate : (2010 ~~ 2013)</li><li>filename : report &amp; creationdate : 2009</li></ul> |
| Exact phrase query specifier | \" *exact-phrase-query* \" | Exact phrase query specifier is used to specify phrase for phrase search. | <ul><li>\"term1 term2 term3\"</li><li>\"computational complexity theory\"</li><li>\"formal language\" AND harrison</li></ul> |
| And operation | *left-query* &amp; *right-query*<br/>*left-query* AND *right-query* | And operation is used to find documents that contain both left query and right query. | <ul><li>term1 &amp; term2</li><li>term1 AND term2</li><li>computational &amp; complexity</li></ul> |
| Or operation | *left-query* &#124; *right-query*<br/>*left-query* &#124;&#124; *right-query*<br/>*left-query* OR *right-query* | Or operation is used to find documents that contain left query, or right query, or both. | <ul><li>term1 &#124; term2</li><li>term1 &#124;&#124; term2</li><li>term1 OR term2</li><li>\"cumulative distribution function\" OR \"cumulative density function\"</li></ul> |
| Not operation | ! *inner-query*<br/>NOT *inner-query* | Not operation is used to find all documents which do not contain inner query. | <ul><li>! term</li><li>NOT term</li><li>author : (Cardano AND NOT Gerolamo)</li></ul> |
| Alias | @*alias-name*<br/>where *alias-name* consists of characters from the ranges a-z and 0-9. | An alias is replaced with the text associated with it in the alias dictionary before the query is parsed. An alias must be separated from the adjacent text by spaces or parentheses. See [Using aliases]({{< ref "search/net/developer-guide/advanced-usage/searching/using-aliases.md" >}}). | <ul><li>@querytext</li><li>@alias1 &amp; @alias2</li><li>(@alias1 OR @alias2) AND term</li></ul> |
| Regular expression specifier | ^*regular-expression* | Regular expression specifier is used to specify query that is regular expression. | <ul><li>^^\[0-9\]{1,5}$</li></ul> |
| Numeric range specifier | *start-number* ~~ *end-number* | Numeric range specifier is used to specify range for numeric range search. | <ul><li>13 ~~ 42</li><li>10000000000 ~~ 100000000000</li></ul> |
| Date range specifier | daterange( *start-date* ~~ *end-date*)<br/>where *start-date* and *end-date* are dates in the format \'yyyy-MM-dd\'. | Date range specifier is used to specify range for date range search. | <ul><li>daterange(2017-09-28 ~~ 2017-11-11)</li></ul> |
| Wildcard | ? - wildcard character (since v.18.12),<br/>?(*N*~*M*) - wildcard character range (since v.18.12),<br/>\**N* - wildcard word (since v.18.1),<br/>\**N*~~*M* - wildcard word range (since v.18.1),<br/>where *N* and *M* are in the range from 0 to 255. | Wildcard characters and wildcard words are used to search for words with unknown characters and phrases with unknown words, respectively. | <ul><li>?ffect - for \'affect\', \'effect\'</li><li>princip?(2\~4) - for \'principal\', \'principle\', \'principles\', \'principally\'</li><li>?(0\~2)sure - for \'assure\', \'ensure\', \'sure\'</li><li>\"First law \*1 thermodynamics\" - for \'First law of thermodynamics\'</li><li>\"Frodo \*1~~5 Pippin\" - for \'Frodo spoke to Pippin\', \'Frodo stripped the blankets from Pippin\'</li></ul> |
| Escape sequence (since v.19.2) | \\*N*<br/>where *N* can be one of<br/>( ) : \" & &#124; ! ^ ~ \* ? \\<br/>\- or -<br/>s (for space character)<br/>\- or -<br/>u*hhhh*<br/>where *h* is hexadecimal digit (for any Unicode character) | The escape sequence allows a special character to be used in a query as it is. Note that an escaped character must be represented in Alphabet in order for it to be indexed as a valid character (not as Separator). | <ul><li>\\& - escaping & character</li><li>\\~ - escaping ~ character</li><li>\\s - escaping space character</li><li>\\u0026 - escaping & character (Unicode code)</li><li>R\\&B - searching for 'R&B' as a whole word (& character must have an appropriate type in Alphabet before indexing)</li></ul> |

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
