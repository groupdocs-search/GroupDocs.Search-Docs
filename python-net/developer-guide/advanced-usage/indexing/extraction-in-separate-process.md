---
id: extraction-in-separate-process
url: search/python-net/extraction-in-separate-process
title: Extraction in separate process
weight: 11
description: "This article describes how to extract data from documents in a separate Python process to protect the indexing process from memory shortage and long extraction."
keywords: extraction in separate process, ExtractionHost, SeparateProcessOptions, out of memory, extraction timeout, Python
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
Extracting data from documents is the most resource-intensive operation of the indexing process. In case of insufficient memory, an exception occurs that completely interrupts the process of indexing a group of documents. To prevent this situation, data can be extracted in a separate process. In this case, insufficient memory when extracting data from one document will lead to a failure in indexing only this document. Indexing of the remaining documents in the group will continue.

Extracting data from documents in a separate process also solves another problem: a document whose extraction exceeds the allocated time limit is interrupted at once.

To extract data from documents in a separate process, you need to configure the appropriate indexing options and create a simple script that the GroupDocs.Search library will run in a separate process as a data extraction service. The script must run with a Python interpreter that has the groupdocs-search-net package installed, as does the main script using the library. Save the code presented in the following listing as `extraction_service.py`.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
# extraction_service.py
import sys

from groupdocs.search import ExtractionHost

# The index starts this script and passes the name of the communication channel
with ExtractionHost(sys.argv[1:]) as host:
    host.run()
```
{{< /tab >}}
{{< /tabs >}}

The [ExtractionHost](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractionhost/) class receives the name of the communication channel from the command line arguments, so the arguments (`sys.argv[1:]`) must be passed to its constructor unchanged. Without them, the constructor raises `ValueError`, so the script is started by the index, not by hand. The [run](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractionhost/run/) method processes extraction requests until the main process stops the service. The license set in the main process is passed to the extraction process automatically; in Python, the extraction service also applies the license named by the `GROUPDOCS_LIC_PATH` environment variable, which it inherits from the main script. Requests for document passwords and extraction errors are passed to the main process.

Custom text extractors added to the index settings are not passed to the extraction process. To use [custom text extractors]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/custom-text-extractors.md" >}}), add them to the [custom_extractors](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractionhost/custom_extractors/) collection of the host before calling the [run](https://reference.groupdocs.com/search/python-net/groupdocs.search/extractionhost/run/) method, for example: `host.custom_extractors.add(LogExtractor())`.

The options for extraction in a separate process are specified in the [SeparateProcessOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/) object available through the [IndexingOptions.separate_process_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/separate_process_options/) or [UpdateOptions.separate_process_options](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/updateoptions/separate_process_options/) property:

*   [extract_in_separate_process](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/extract_in_separate_process/) enables extraction in a separate process. The default value is `False`. Note that in this mode, data are extracted in only one thread: the number of threads specified in the [threads](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/indexingoptions/threads/) property is ignored.
*   [assembly_path](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/assembly_path/) specifies the path to the assembly of the extraction service. From Python, it is the path to the launcher of the extraction service script (see below).
*   [timeout](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/timeout/) specifies the maximum time to wait for the result of data extraction from one document. The default value is 10 minutes.
*   [use_dotnet_to_start_process](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/use_dotnet_to_start_process/) specifies whether the process is started with the `dotnet` command, which runs the assembly specified by [assembly_path](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/assembly_path/). The default value is `True`, which starts a .NET console application. Set it to `False` to start an executable file directly; in this case, [assembly_path](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/assembly_path/) must specify the executable file. A Python extraction service is started this way, through its launcher.

The index starts the extraction service as an executable file and passes the name of the channel as its only argument. A Python script cannot be started this way on every platform, so the example below first writes a launcher next to the script: a `.cmd` file on Windows, a shell script on Linux and macOS. The launcher runs `extraction_service.py` with the interpreter of the main script (`sys.executable`). The example then sets the indexing options for data extraction in a separate process and indexes the documents. The extraction service loads the library in its own process, which takes about as long as the first call to the library in the main script; this start is not counted in the timeout of a document. Keep [use_dotnet_to_start_process](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/use_dotnet_to_start_process/) set to `False` and [assembly_path](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/separateprocessoptions/assembly_path/) set to the launcher: if the launcher file does not exist, the error is reported through the [error_occurred](https://reference.groupdocs.com/search/python-net/groupdocs.search.events/eventhub/error_occurred/) event and the documents are not indexed, but if no `assembly_path` is set, or if `dotnet` is asked to run the script, the indexing operation waits for a service that never connects.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import datetime
import os
import sys

from groupdocs.search import Index
from groupdocs.search.options import IndexingOptions

# The index starts the extraction service as an executable file,
# so a launcher runs extraction_service.py with this Python interpreter
service_path = os.path.abspath("./extraction_service.py")
if os.name == "nt":
    launcher_path = os.path.abspath("./extraction_service.cmd")
    with open(launcher_path, "w") as f:
        f.write('@"%s" "%s" %%*\n' % (sys.executable, service_path))
else:
    launcher_path = os.path.abspath("./extraction_service.sh")
    with open(launcher_path, "w") as f:
        f.write('#!/bin/sh\nexec "%s" "%s" "$@"\n' % (sys.executable, service_path))
    os.chmod(launcher_path, 0o755)

# Creating an index in the specified folder
index = Index("./index")

# Setting indexing options for data extraction in a separate process
options = IndexingOptions()
options.separate_process_options.extract_in_separate_process = True
options.separate_process_options.assembly_path = launcher_path
options.separate_process_options.use_dotnet_to_start_process = False
options.separate_process_options.timeout = datetime.timedelta(minutes=1)

# Indexing documents from the specified folder
index.add("./documents", options)

result = index.search("Einstein")
print("Documents found:", result.document_count)
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/Einstein.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/extraction-in-separate-process/documents/Einstein.txt), [documents/Newton.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/indexing/extraction-in-separate-process/documents/Newton.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extraction-in-separate-process.txt" >}}  
```text
Documents found: 2
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/indexing/extraction-in-separate-process/extraction_in_separate_process/extraction-in-separate-process.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may find more code examples in our GitHub repository:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide a free online document search app. You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our [Free Online Document Search App](https://products.groupdocs.app/search).
