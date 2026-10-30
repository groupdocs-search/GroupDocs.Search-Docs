---
id: keyboard-layout-correction
url: search/python-net/keyboard-layout-correction
title: Keyboard layout correction
weight: 10
description: "This article describes the keyboard layout correction feature, which fixes words in search queries typed in a wrong keyboard layout."
keywords: Keyboard layout correction
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
When entering search queries, users of your software may make input errors, forgetting to switch the desired keyboard layout. For example, entering the word 'Einstein' in the Russian keyboard layout will result in the word 'Уштыеушт' appearing.

To automatically fix such misprints, the keyboard layout correction feature can be used. To enable this feature, you must set the True value for the [enabled](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/keyboardlayoutcorrectoroptions/enabled/) property in the search options. By default, this feature is disabled.

Keyboard layout correction cannot be combined with case-sensitive search. If both are enabled, the search returns an empty result and the error is reported through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event (see [Search options]({{< ref "search/python-net/developer-guide/advanced-usage/searching/search-options.md#incompatible-options" >}})).

The following example demonstrates the use of the keyboard layout correction feature.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import SearchOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Creating a search options object
options = SearchOptions()
options.keyboard_layout_corrector.enabled = True  # Enabling keyboard layout correction

# Search for the word 'Уштыеушт' gives documents containing the word 'Einstein'
result = index.search("Уштыеушт", options)

# Printing the words found in each document
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path), document.terms)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/einstein-biography.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/keyboard-layout-correction/documents/einstein-biography.txt), [documents/quantum-theory.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/keyboard-layout-correction/documents/quantum-theory.txt), [documents/nobel-prize.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/keyboard-layout-correction/documents/nobel-prize.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "keyboard-layout-correction.txt" >}}  
```text
Documents found: 2
    einstein-biography.txt ['einstein']
    nobel-prize.txt ['einstein']
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/keyboard-layout-correction/keyboard_layout_correction/keyboard-layout-correction.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
