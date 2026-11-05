---
id: how-to-run-examples
url: search/python-net/how-to-run-examples
title: How to Run Examples
weight: 7
description: "How to clone, set up, and run the GroupDocs.Search for Python via .NET example suite - locally, with a license, or in Docker."
keywords: "groupdocs search python, examples, run examples, docker, ci"
productName: GroupDocs.Search for Python via .NET
hideChildren: False
toc: True
structuredData:
  showOrganization: True
  application:
    name: Document Search
    description: Index documents and search them natively with high performance using Python language and GroupDocs.Search for Python via .NET
    productCode: search
    productPlatform: python-net
  howTo:
    name: How to run Search examples
    description: Learn how to run GroupDocs.Search examples using Python
    steps:
      - name: Clone repository
        text: Clone the repository with Search examples from GitHub
      - name: Install package
        text: Install the GroupDocs.Search package with pip
      - name: Run examples
        text: Execute run_all_examples.py to run the examples
---

The complete project [GroupDocs.Search for Python via .NET](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET) with code examples and sample files is hosted on GitHub. Every example of this documentation is there as a script, next to its input files.

## Prerequisites

- [Python 3.5-3.14](https://www.python.org/downloads/), 64-bit.
- On Linux, ICU and `libgdiplus` (`libicu-dev libgdiplus`). On macOS: `mono-libgdiplus` (via Homebrew). See [System Requirements]({{< ref "search/python-net/getting-started/system-requirements.md" >}}) for what each one is needed for.

## Get the code

```bash
git clone https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET.git
cd ./GroupDocs.Search-for-Python-via-.NET/Examples
```

## Project structure

```
Examples/
├── run_all_examples.py     # Runs every example and prints a pass/fail summary
├── requirements.txt
├── product-overview/       # The quick examples of the product overview
├── licensing/              # Set a license from a file, a stream, or a metered key; the evaluation limits
├── getting-started/
│   ├── hello-world/        # Index a folder of documents and search it
│   └── quick-start-guide/  # Phrase search, fuzzy search and highlighting
└── developer-guide/
    ├── basic-usage/        # Build an index and queries, read search results, the index lifecycle
    ├── advanced-usage/     # Indexing, searching, dictionaries, index events and Python callbacks
    └── migration-notes/
```

The folders follow this documentation: each page's examples are in the folder of that page, with the files they read. The search network examples ([Scaling]({{< ref "search/python-net/developer-guide/advanced-usage/scaling/_index.md" >}})) are not in the repository, because they need several nodes running at once.

## Set up

```bash
pip install -r requirements.txt
```

To run the examples with the full feature set, apply a license. The simplest way is the `GROUPDOCS_LIC_PATH` environment variable, which `run_all_examples.py` reads and the package applies at the first call into the engine:

{{< tabs "set-license" >}}
{{< tab "Windows" >}}
```batch
set GROUPDOCS_LIC_PATH=C:\path\to\GroupDocs.Search.lic
```
{{< /tab >}}
{{< tab "Linux / macOS" >}}
```bash
export GROUPDOCS_LIC_PATH=/path/to/GroupDocs.Search.lic
```
{{< /tab >}}
{{< /tabs >}}

Without a license the suite still runs, in evaluation mode: an index takes at most 100 documents, and only the first 10 found documents of a search can be read (see [Licensing and evaluation]({{< ref "search/python-net/getting-started/licensing-and-subscription.md" >}})).

## Run the examples

Run the full suite, which prints a pass/fail summary at the end:

```bash
python run_all_examples.py
```

You can also run any single example by navigating to its folder and running the script directly. Each example uses paths relative to its own folder, and writes its index and its results next to the script:

```bash
cd getting-started/hello-world
python hello_world.py
```

The examples of one page share a folder and an index, and some read what an earlier one wrote, so run them in the order of the page, as `run_all_examples.py` does. Delete the index folders (`index`, `index1`, ...) to start again from empty indexes.

## Run with Docker

The repository ships a `Dockerfile` that builds a Linux image with Python 3.13, the system packages the library needs on Linux (ICU and `libgdiplus`), and the package preinstalled from PyPI.

```bash
# Build the image
docker build -t search-examples .

# Run unlicensed (evaluation mode)
docker run --rm search-examples

# Run with a license mounted from the host
docker run --rm \
    -v /path/to/license:/lic:ro \
    -e GROUPDOCS_LIC_PATH=/lic/your-license.lic \
    search-examples
```

## Continuous integration

The `.github/workflows/` directory contains a workflow that runs the full example suite on every push, on `ubuntu-latest` with Python 3.13. The same run is reproducible locally via the `Dockerfile` above.

## Troubleshooting

- **"Couldn't find a valid ICU package"**, with the Python process ending abruptly (Linux) - install ICU (`libicu-dev` on Debian and Ubuntu). Do not set `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT`.
- **ODP and PPT files missing from the results** (Linux/macOS) - install `libgdiplus` (`sudo apt install libgdiplus`, or `brew install mono-libgdiplus` on macOS). Without it the index's `error_occurred` event reports "The type initializer for 'Gdip' threw an exception" for these files and indexes the rest.
- **"Number of indexed documents is restricted"**, or `LicenseRestrictionException: Only first 10 results can be returned` - the evaluation limits. Apply a license (see above); a free 30-day [temporary license](https://purchase.groupdocs.com/temporary-license) is available.
- **`LicenseError` at the first call** - `GROUPDOCS_LIC_PATH` names a file that is missing, expired or for another product. The message gives the engine's reason.
- **The first example takes 15-30 seconds** (Windows, with a license) - that is the engine's license check, which runs once per process.
- **The output differs from the output tab** - the tabs come from a run on Windows, and parts of the output depend on where and when an example runs:
  - the order of the documents: the index adds the files of a folder in the order in which the file system lists them, which is name order on Windows but not on Linux;
  - times, durations, index sizes and the identifier of an index;
  - dates that .NET writes in the local time zone and culture: `1/1/2017 12:00:00 AM` on a Windows set to US English, `01/01/2017 00:00:00` on a Linux without a locale;
  - counts that include the `filename` field, which holds the whole path, so a longer folder path adds terms.

## Contribute

If you would like to add or improve an example, we encourage you to contribute to the project. All examples in this repository are open source and can be freely used in your own applications. Fork the repository, edit the source code, and open a pull request.
