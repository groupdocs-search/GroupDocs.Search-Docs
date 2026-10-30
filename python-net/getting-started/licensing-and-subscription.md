---
id: licensing-and-evaluation
url: search/python-net/licensing-and-evaluation
title: Licensing and evaluation
weight: 5
keywords: free, free trial, subscription, evaluation, evaluation limitations, license, metered license, set_license, set_metered_key, GROUPDOCS_LIC_PATH, LicenseError, groupdocs search python
description: "GroupDocs.Search for Python via .NET offers flexible licensing options, including a Free Trial with evaluation limits and a 30-day Temporary License for evaluation purposes."
productName: GroupDocs.Search for Python via .NET
hideChildren: False
toc: True
aliases:
    - /search/python-net/licensing-and-subscription/
    - /search/python-net/evaluation-limitations-and-licensing/
---

To explore the system effectively, you may want immediate access to the API. GroupDocs.Search simplifies this by offering various purchase plans, along with a Free Trial and a 30-day Temporary License for evaluation.

{{< alert style="info" >}}

To learn more about licensing options, purchasing, and evaluation policies, refer to the [Purchase Policies and FAQ](https://purchase.groupdocs.com/policies) section.

{{< /alert >}}

## Purchased License

After purchasing GroupDocs.Search for Python via .NET, you will receive a license file that unlocks the full functionality of the API. A few rules apply:

- Apply the license **only once** per process, in your start-up code.
- Apply it **before** creating any `Index` or other GroupDocs.Search object.
- A license can be applied from a file path, from a binary stream (handy when the license is an embedded resource), from an environment variable, or as a [Metered License](https://purchase.groupdocs.com/faqs/licensing/metered/) that bills by usage.

The license file contains details such as the product name, the number of developers it is licensed to, and the subscription expiry date. It carries a digital signature, so do not modify the file: even an inadvertent extra line break invalidates it.

Use the [set_license](https://reference.groupdocs.com/search/python-net/groupdocs.search/license/) method to license the component. Calling it more than once is harmless - it simply wastes a little processor time. On Windows, the first license check in a process takes 15-30 seconds; later calls are immediate.

### Set a license from a file

The example below applies a license from a file path. It reads the path from the `GROUPDOCS_LIC_PATH` environment variable and falls back to a local `GroupDocs.Search.lic` file. A relative path is resolved against the current working directory of the process, which may differ from your script's folder (for example, in a service or a web application), so an absolute path is recommended:

{{< tabs "code-example-set-license-from-file" >}}
{{< tab "set_license_from_file.py" >}}
```python
import os

from groupdocs.search import License

def set_license_from_file():
    # Resolve the license path from the environment, with a local fallback
    license_path = os.environ.get("GROUPDOCS_LIC_PATH", "./GroupDocs.Search.lic")

    # Apply the license before using any search features
    if os.path.exists(license_path):
        License().set_license(license_path)
        print("License set successfully.")
    else:
        print("License file not found. Running in evaluation mode.")

if __name__ == "__main__":
    set_license_from_file()
```
{{< /tab >}}
{{< /tabs >}}

### Set a license from a stream

You can also apply a license from any readable binary stream - useful when the license ships as an embedded resource:

{{< tabs "code-example-set-license-from-stream" >}}
{{< tab "set_license_from_stream.py" >}}
```python
import os

from groupdocs.search import License

def set_license_from_stream():
    # Resolve the license path from the environment, with a local fallback
    license_path = os.environ.get("GROUPDOCS_LIC_PATH", "./GroupDocs.Search.lic")

    # Apply the license from an open binary stream
    if os.path.exists(license_path):
        with open(license_path, "rb") as stream:
            License().set_license(stream)
        print("License set successfully.")
    else:
        print("License file not found. Running in evaluation mode.")

if __name__ == "__main__":
    set_license_from_stream()
```
{{< /tab >}}
{{< /tabs >}}

### Set a license from an environment variable

Set `GROUPDOCS_LIC_PATH` to the license file's path, or to a URL that serves it, and the license is applied at the first call into the engine, with no licensing code:

{{< tabs "env-license">}}
{{< tab "Windows" >}}
```ps
$env:GROUPDOCS_LIC_PATH = "C:\licenses\GroupDocs.Search.lic"
```
{{< /tab >}}
{{< tab "Linux / macOS" >}}
```bash
export GROUPDOCS_LIC_PATH="/opt/licenses/GroupDocs.Search.lic"
```
{{< /tab >}}
{{< /tabs >}}

A license that cannot be applied (a missing file, an expired subscription, a license for another product) raises `LicenseError` from the first call into the engine on, rather than letting the process run in evaluation mode unnoticed. The message names the variable and the engine's reason. Set `GROUPDOCS_LIC_STRICT=0` to get a `LicenseWarning` and evaluation mode instead.

### Set a metered license

A Metered License lets you pay for what you use. Pass the public and private keys to [set_metered_key](https://reference.groupdocs.com/search/python-net/groupdocs.search/metered/) before using the API, then read the consumption with `Metered.get_consumption_quantity()` and `Metered.get_consumption_credit()`. The example reads the keys from two environment variables:

{{< tabs "code-example-set-metered-license" >}}
{{< tab "set_metered_license.py" >}}
```python
import os

from groupdocs.search import Metered

def set_metered_license():
    public_key = os.environ.get("GROUPDOCS_METERED_PUBLIC_KEY")
    private_key = os.environ.get("GROUPDOCS_METERED_PRIVATE_KEY")

    # Skip the call until real keys are supplied
    if not (public_key and private_key):
        print("Set GROUPDOCS_METERED_PUBLIC_KEY and GROUPDOCS_METERED_PRIVATE_KEY to apply a metered license.")
        return

    # Apply the metered keys before using any search features
    Metered().set_metered_key(public_key, private_key)
    print("Quantity:", Metered.get_consumption_quantity())
    print("Credit:", Metered.get_consumption_credit())

if __name__ == "__main__":
    set_metered_license()
```
{{< /tab >}}
{{< /tabs >}}

### Changing the license file name

You are not required to keep the license file name as `GroupDocs.Search.lic`. You can rename it to any preferred name and use that name when applying the license in your application.

### "Cannot find license filename" exception

When you download the license from the GroupDocs website it is saved as `GroupDocs.Search.lic`. However, some web browsers may automatically append `.xml`, resulting in `GroupDocs.Search.lic.xml`.

If your Windows settings are configured to hide file extensions (the default), the file may still appear as `GroupDocs.Search.lic` in File Explorer even though the actual name is `GroupDocs.Search.lic.xml`. This discrepancy can cause `set_license` to raise an exception. To fix it, manually rename the file to remove the `.xml` extension, or disable "Hide extensions for known file types" in Windows.

## How to evaluate GroupDocs.Search

You can evaluate GroupDocs.Search for Python via .NET without purchasing a license by using one of the following options.

### Free Trial

The evaluation version is identical to the purchased one; it becomes fully licensed once you set a license. Its usage and functionality are the same, but without a license the following limits apply.

#### Indexing limits

*   The total number of indexed documents in one index or in all indexes of an index repository cannot exceed 100.
*   If adding documents would exceed this number, only the documents that fit into the limit are indexed. `add()` does not raise: the index's `error_occurred` event reports "Number of indexed documents is restricted".
*   All changed documents can be updated in an index during [update](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/update/).
*   New documents can be added to an index during update only while the total number of indexed documents is less than 100.
*   Indexes can be merged only if the total number of documents in the current index and in the merged indexes does not exceed 100. Otherwise, the [merge](https://reference.groupdocs.com/search/python-net/groupdocs.search/index/merge/) method raises `LicenseRestrictionException`.

#### Search limits

Only the first 10 found documents can be retrieved from a search result. The `document_count` property returns the total number of found documents, but `get_found_document` raises `LicenseRestrictionException` ("Only first 10 results can be returned") for a document number of 10 or greater. Therefore, in the evaluation mode, a loop over found documents must not exceed 10 iterations:

{{< tabs "code-example-searching-limitations" >}}
{{< tab "searching_limitations.py" >}}
```python
import os

from groupdocs.search import Index

def searching_limitations():
    with Index("./index") as index:
        index.add("./documents")

        result = index.search("contract")
        # Only the first 10 found documents are available in the evaluation mode
        count = min(result.document_count, 10)
        for i in range(count):
            document = result.get_found_document(i)
            print(os.path.basename(document.document_info.file_path))

if __name__ == "__main__":
    searching_limitations()
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/loan-contract.txt](/search/python-net/_sample_files/getting-started/licensing-and-subscription/documents/loan-contract.txt), [documents/digitisation-contract.txt](/search/python-net/_sample_files/getting-started/licensing-and-subscription/documents/digitisation-contract.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "searching-limitations.txt" >}}  
```text
digitisation-contract.txt
loan-contract.txt
```
[Download full output](/search/python-net/_output_files/getting-started/licensing-and-subscription/searching_limitations/searching-limitations.txt)
{{< /tab >}}
{{< /tabs >}}

#### Search network limits

A [search network]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/_index.md" >}}) can contain only 1 shard and 1 extractor. If the configuration of the network contains more shards or extractors, the methods of the indexer and searcher nodes raise an exception.

### Temporary License

To experience the complete features of GroupDocs.Search without the limitations of the Free Trial, you can request a 30-day ["Temporary License"](https://purchase.groupdocs.com/temporary-license).
