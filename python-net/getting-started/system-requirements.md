---
id: system-requirements
url: search/python-net/system-requirements
title: System Requirements
linkTitle: System Requirements
weight: 3
description: "System requirements for GroupDocs.Search for Python via .NET - 64-bit Python 3.5-3.14 on Windows, Linux (glibc 2.27+) and macOS 13+, with ICU and libgdiplus on Linux."
keywords: GroupDocs.Search for Python via .NET, system requirements, supported operating systems, Python versions, Windows, Linux, macOS, glibc, ICU, libgdiplus, pip
productName: GroupDocs.Search for Python via .NET
hideChildren: False
toc: True
---

{{< alert style="info" >}}
GroupDocs.Search for Python via .NET ships as a self-contained wheel that bundles the .NET runtime it needs. No Microsoft Office, third-party document software, .NET install or Mono install is required. On Linux and macOS a few system packages are needed; see [Platform Dependencies](#platform-dependencies). To install the library, follow the steps in the [Installation]({{< ref "search/python-net/getting-started/installation.md" >}}) guide.
{{< /alert >}}

## Supported Operating Systems

GroupDocs.Search for Python via .NET runs on Windows, Linux, and macOS. The package ships as a platform-specific `.whl`, and `pip` picks the correct one automatically.

### Windows

* Microsoft Windows 10 (x64)
* Microsoft Windows 11 (x64)
* Microsoft Windows Server 2016, 2019, 2022 and 2025

The Windows wheel is 64-bit only: use a 64-bit Python.

### Linux

* Any **x86-64** distribution with **glibc 2.27 or newer** - for example Ubuntu 18.04+, Debian 10+, RHEL 8+. The embedded .NET runtime needs glibc 2.27, and the wheel's `manylinux_2_27_x86_64` tag says so, so pip refuses an older system.

### macOS

* macOS 13 (Ventura) and later - Intel and Apple Silicon (M-series). The wheels are tagged `macosx_13_0_*`: the ONNX Runtime library they bundle requires macOS 13. Microsoft supports .NET 10 on macOS 14 and later.

## Python Version

GroupDocs.Search for Python via .NET supports **Python 3.5 through 3.14**, 64-bit. The wheel uses the `py3-none-{platform}` tag, meaning it works with any Python 3.x version in that range without per-version rebuilds.

| Python Version | Supported |
| --- | :---: |
| 3.5  | Yes |
| 3.6  | Yes |
| 3.7  | Yes |
| 3.8  | Yes |
| 3.9  | Yes |
| 3.10 | Yes |
| 3.11 | Yes |
| 3.12 | Yes |
| 3.13 | Yes |
| 3.14 | Yes |

## Package Manager

GroupDocs.Search for Python via .NET is distributed via [PyPI](https://pypi.org/project/groupdocs-search-net/) and the [GroupDocs Releases website](https://releases.groupdocs.com/search/python-net/):

```bash
pip install groupdocs-search-net
```

Until PyPI carries the 26.10 wheels, install a wheel downloaded from the releases website, as described on the [Installation]({{< ref "search/python-net/getting-started/installation.md" >}}) page. One wheel is published per platform:

| Platform | Wheel suffix |
| --- | --- |
| Windows 64-bit | `py3-none-win_amd64.whl` |
| Linux x64 (glibc) | `py3-none-manylinux_2_27_x86_64.whl` |
| macOS Apple Silicon | `py3-none-macosx_13_0_arm64.whl` |
| macOS Intel | `py3-none-macosx_13_0_x86_64.whl` |

`pip` 20.3 or newer picks the right wheel for your platform; older versions do not recognise these tags, so upgrade with `python -m pip install --upgrade pip` (on Python 3.5, `python -m pip install "pip==20.3.4"`). On an Intel Mac, a Python built against a pre-11 macOS SDK reports its system as macOS 10.16 - there use pip 24.1 or newer, or run `SYSTEM_VERSION_COMPAT=0 pip install groupdocs-search-net`.

## Platform Dependencies

### Linux

Install **ICU** and **`libgdiplus`**:

```bash
sudo apt-get update
sudo apt-get install -y libicu-dev libgdiplus
```

What each package is for, measured on the 26.10 engine:

- **ICU** (`libicu-dev`, any version your distribution ships) - required. The .NET runtime needs it for text processing: without it, the first call into the engine aborts the Python process with `Couldn't find a valid ICU package`. Do not set `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1` to work around it.
- **`libgdiplus`** - presentations. Without it, ODP and PPT files are left out of the index, and the index's [error_occurred]({{< ref "search/python-net/developer-guide/advanced-usage/creating-an-index/search-index-events.md" >}}) event reports `The type initializer for 'Gdip' threw an exception` or a `NullReferenceException`. Every other format indexes without it, PPTX included.

No fonts are needed: indexing and searching on an image with ICU alone, without fontconfig or any font files, gives the same results.

### macOS

Install `mono-libgdiplus` using [Homebrew](https://brew.sh/), for presentations, as on Linux:

```bash
brew install mono-libgdiplus
```

### Windows

No extra dependencies - the wheel is self-contained.

### Text in images

Recognizing text in images (OCR) requires an OCR engine connected through an OCR connector; see [OCR support]({{< ref "search/python-net/developer-guide/advanced-usage/indexing/ocr-support.md" >}}).

## Performance Notes

* The first call into the engine takes about a second.
* On Windows, when a license is applied, the first call in a process takes 15-30 seconds: that is the engine's license check, which runs once per process. Keep one process for many operations.

## Development Environments

Any editor or IDE that runs CPython works, for example Visual Studio Code, PyCharm and Jupyter. Type stubs (`.pyi`) ship in the wheel, so editors complete class and member names.
