---
id: logging
url: search/python-net/logging
title: Logging
weight: 19
description: "This article explains how to create and assign a logger of an index and how to implement a custom logger in Python."
keywords: logging, logger, FileLogger, ILogger, custom logger, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page contains information on creating and assigning a logger of an index, as well as on the implementation of a custom logger.

## Use of standard file logger

In order to save information about events and errors in an index into a file, you should use the standard file logger or your own custom logger. It is important to remember that each time you open an existing index, you must create and assign an instance of the logger again, because the logger is not saved. The logger receives, among other things, the messages that the index reports while it is being opened, for example, about an interrupted indexing operation (see [Checking index integrity]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/checking-index-integrity.md" >}})). The same code can be used to create a new index and to open an existing one: when an existing index is opened, the logger is taken from the index settings passed to the constructor, and the other index settings are loaded from the index.

The following example demonstrates the use of the standard file logger. The constructor parameters of the [FileLogger](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/filelogger/) class are the path to the log file and its maximum size in MB. The maximum size should be in the range from 0.1 to 1000 MB; the value is not checked. When the log file exceeds the maximum size, the older part of the log is removed, and the most recent half of the maximum size is kept. The example prints the log file at the end.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index, IndexSettings
from groupdocs.search.common import FileLogger

query = "Einstein"
log_path = "./output/log.txt"
os.makedirs("./output", exist_ok=True)

settings = IndexSettings()
settings.logger = FileLogger(log_path, 4.0)  # Specifying the path to the log file and a maximum length of 4 MB

index = Index("./index", settings)  # Creating or loading an index from the specified folder

index.add("./documents")  # Indexing documents from the specified folder

result = index.search(query)  # Search in index

with open(log_path, encoding="utf-8") as f:
    print(f.read())
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/logging/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/logging/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "log.txt" >}}  
```text
2026-10-29 19:05:40.145	Index created on disk.
2026-10-29 19:05:40.146	Indexing started
2026-10-29 19:05:40.282	Indexing finished
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/logging/use_of_standard_file_logger/log.txt)
{{< /tab >}}
{{< /tabs >}}

## Implementing custom logger

Sometimes you may need to implement your own logger, for example, to display the log in the console. A custom logger implements the [ILogger](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/ilogger/) interface. In Python, it is a plain class with the methods of the interface; do not derive it from the `ILogger` class. The [error](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/ilogger/error/) method logs errors, and the [trace](https://reference.groupdocs.com/search/python-net/groupdocs.search.common/ilogger/trace/) method logs events. An example implementation of such a logger is presented below.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
class ConsoleLogger(object):
    def error(self, message):
        print("Error: " + message)

    def trace(self, message):
        print(message)
```
{{< /tab >}}
{{< /tabs >}}

The custom logger is assigned in the same way as the standard one. A function that takes the level (`"error"` or `"trace"`) and the message can be assigned as a logger too, for example `settings.logger = lambda level, message: print(level, message)`.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
from groupdocs.search import Index, IndexSettings


class ConsoleLogger(object):
    def error(self, message):
        print("Error: " + message)

    def trace(self, message):
        print(message)


settings = IndexSettings()
settings.logger = ConsoleLogger()  # Assigning the custom logger

index = Index("./index", settings)  # Creating or loading an index from the specified folder
```
{{< /tab >}}
{{< tab "implementing-custom-logger-2.txt" >}}  
```text
Index created on disk.
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/logging/implementing_custom_logger_2/implementing-custom-logger-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
