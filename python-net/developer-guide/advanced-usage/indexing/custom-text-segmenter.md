---
id: custom-text-segmenter
url: search/python-net/custom-text-segmenter
title: Custom text segmenter
weight: 5
description: "This article explains how to use an external library to split text into words during indexing in Python by implementing the IWordSplitter interface."
keywords: custom text segmenter, word splitter, text splitter, IWordSplitter, jieba, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---

Some languages, in particular hieroglyphic languages, typically require special tools to split text into words.
This is necessary due to the lack of spaces between words, polysemantic phrases, set expressions, and complex lexical rules.

Currently, the GroupDocs.Search library does not contain such special algorithms for splitting Chinese, Japanese, and Korean text into words. However, it provides the ability to use external custom algorithms for this. To connect an external text segmentation library, you must implement the [IWordSplitter](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/iwordsplitter/) interface and pass an object implementing this interface to the [word_splitter](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/fileindexingeventargs/word_splitter/) property of the [file_indexing](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/file_indexing/) event arguments. In Python, the word splitter is a plain class with a `split(self, text)` method that returns a list (or another iterable) of words; do not derive it from the `IWordSplitter` class. Since the interface has one method, a function that takes the text works too.

The words returned by the word splitter are checked against the [tokenization dictionary]({{< ref "search/python-net/developer-guide/advanced-usage/managing-dictionaries/tokenization-dictionary.md" >}}) of the index ([dictionaries.tokenization_dictionary](https://reference.groupdocs.com/search/python-net/groupdocs.search.dictionaries/dictionaryrepository/tokenization_dictionary/)): a word is indexed only if it contains at least one character that is not a separator in this dictionary; other words, for example punctuation marks, are skipped. The tokenization dictionary has the same structure as the alphabet, and by default it also defines the CJK ideographs as letters. Like the alphabet, it can be changed only while the index contains no documents. Note also that words longer than 80 characters are not indexed.

The code example below demonstrates how to use the external library [jieba](https://pypi.org/project/jieba/) for Chinese text segmentation; install it with `pip install jieba`. If the library is not installed, the example splits each run of ideographs into overlapping two-character words instead, a simple way to index Chinese text without a dictionary.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import re

from groupdocs.search import Index

try:
    import jieba  # pip install jieba
except ImportError:
    jieba = None


# Implementing custom word splitter
class JiebaWordSplitter(object):
    def split(self, text):
        if jieba is not None:
            return jieba.lcut(text, cut_all=False)
        # Without jieba: each run of ideographs is split into overlapping two-character words
        words = []
        for run in re.findall("[\u4e00-\u9fff]+|\\w+", text):
            if "\u4e00" <= run[0] <= "\u9fff" and len(run) > 1:
                words.extend(run[i:i + 2] for i in range(len(run) - 1))
            else:
                words.append(run)
        return words


# Creating an index in the specified folder
index = Index("./index")

# Using Jieba segmenter to break text into words
jieba_word_splitter = JiebaWordSplitter()


def on_file_indexing(sender, args):
    if args.document_full_path.endswith("Chinese.txt"):
        # We know that the text in this document is in Chinese
        args.word_splitter = jieba_word_splitter


index.events.file_indexing += on_file_indexing

# Indexing documents
index.add("./documents")

# Searching in the index
query = "考虑"  # Consider
result = index.search(query)
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/custom-text-segmenter/documents/Einstein.txt), [documents/Chinese.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/custom-text-segmenter/documents/Chinese.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "custom-text-segmenter.txt" >}}  
```text
Documents found: 1
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/custom-text-segmenter/custom_text_segmenter/custom-text-segmenter.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
