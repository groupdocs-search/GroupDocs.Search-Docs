---
id: custom-text-extractors
url: search/java/custom-text-extractors
title: Custom text extractors
weight: 3
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
GroupDocs.Search supports indexing of many document formats. But there is also the possibility to implement support for any format other than the existing ones.

The following example demonstrates how to implement a custom text extractor.



```java
public class LogExtractor implements IFieldExtractor
{
    private final String[] extensions = new String[] {
        ".log"
    };
 
    public final String[] getExtensions() {
        return extensions;
    }
 
    public final DocumentField[] getFields(String filePath) {
        File file = new File(filePath);
        DocumentField[] fields = new DocumentField[] {
            new DocumentField("FileName", file.getAbsolutePath()),
            new DocumentField("Content", extractContent(filePath)),
        };
        return fields;
    }
 
    private String extractContent(String filePath) {
        StringBuilder result = new StringBuilder();
        try {
            List<String> lines = Files.readAllLines(Paths.get(filePath), StandardCharsets.UTF_8);
            for (int i = 0; i < lines.size(); i++) {
                String line = lines.get(i);
                String processedLine = line.substring(12);
                result.append(processedLine);
            }
        } catch (IOException ex) {
            throw new RuntimeException(ex);
        }
        return result.toString();
    }
}
```

The next example shows how to use the custorm extractor for indexing.



```java
String indexFolder = "c:\\MyIndex\\"; // Specify path to the index folder
String documentsFolder = "c:\\MyDocuments\\"; // Specify path to a folder containing documents to search
 
IndexSettings settings = new IndexSettings();
settings.getCustomExtractors().addItem(new LogExtractor()); // Adding custom text extractor to the index settings
 
Index index = new Index(indexFolder, settings); // Creating or loading an index
 
index.add(documentsFolder); // Indexing documents from the specified folder
```

Note that custom extractors are not saved in an index and must be created and added each time the index is created or loaded. However, the same code can be used to create a new index and open an existing one. In this case, when opening an existing index, custom extractors from the index settings passed to the constructor will be used, the remaining index settings will be loaded from disk.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
