---
id: nesting-search-queries-in-object-form
url: search/java/nesting-search-queries-in-object-form
title: Nesting search queries in object form
weight: 11
description: "This article gives the knowledge about nesting search queries in object form using Java search API."
keywords: Nesting search queries in object form
productName: GroupDocs.Search for Java
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

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
