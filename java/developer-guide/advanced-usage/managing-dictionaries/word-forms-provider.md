---
id: word-forms-provider
url: search/java/word-forms-provider
title: Word forms provider
weight: 9
description: "This article gives the knowledge of the API methods which can be used to perform operations about word forms provider interface using Java."
keywords: word forms provider
productName: GroupDocs.Search for Java
hideChildren: False
---
The [IWordFormsProvider](https://reference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/IWordFormsProvider) interface is designed to implement a word forms provider for searching by word forms. For information on searching by word forms, see [Search for different word forms]({{< ref "search/java/developer-guide/advanced-usage/searching/search-for-different-word-forms.md" >}}) page.

The [IWordFormsProvider](https://reference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/IWordFormsProvider) interface contains only one [getWordForms](https://reference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/IWordFormsProvider#getWordForms(java.lang.String)) method, which returns various forms for the word passed as an argument. An example implementation of a simple provider of word forms is presented below.



```java
public class SimpleWordFormsProvider implements IWordFormsProvider {
    public final String[] getWordForms(String word)     {
        ArrayList<String> result = new ArrayList<String>();
 
        // Assume that the input word is in the plural, then we add the singular
        if (word.length() > 2 &&
            word.toLowerCase().endsWith("es")) {
            result.add(word.substring(0, word.length() - 2));
        }
        if (word.length() > 1 &&
            word.toLowerCase().endsWith("s")) {
            result.add(word.substring(0, word.length() - 1));
        }
 
        // Then assume that the input word is in the singular, we add the plural
        if (word.length() > 1 &&
            word.toLowerCase().endsWith("y")) {
            result.add(word.substring(0, word.length() - 1).concat("is"));
        }
        result.add(word.concat("s"));
        result.add(word.concat("es"));
        // All rules are implemented in the EnglishWordFormsProvider class
 
        return result.toArray(new String[0]);
    }
}
```

By default, the [EnglishWordFormsProvider](https://reference.groupdocs.com/search/java/com.groupdocs.search.dictionaries/EnglishWordFormsProvider) class is used, which for English generates various forms of nouns, adjectives, pronouns, verbs, etc. An example of setting a custom provider of word forms is presented below.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Setting the custom word forms provider instance
index.getDictionaries().setWordFormsProvider(new SimpleWordFormsProvider());
 
// Creating a search options instance
SearchOptions options = new SearchOptions();
options.setUseWordFormsSearch(true); // Enabling search for word forms
 
// Searching in the index
SearchResult result = index.search("relative", options);
 
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

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
