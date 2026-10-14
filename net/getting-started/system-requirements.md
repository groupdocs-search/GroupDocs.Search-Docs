---
id: system-requirements
url: search/net/system-requirements
title: System Requirements
weight: 4
description: "GroupDocs.Search for .NET runs on 32-bit and 64-bit Windows and Linux with .NET Framework 4.6.2 or later, .NET Core 3.x or .NET 5 and later."
keywords: system requirements, supported operating systems, supported frameworks, .NET Framework, .NET Standard, .NET, Visual Studio
productName: GroupDocs.Search for .NET
hideChildren: False
---
## Overview

GroupDocs.Search for .NET does not require any external software or third party tool to be installed. Just follow one of the ways described on the [Installation]({{< ref "search/net/getting-started/installation.md" >}}) page. Recognizing text in images (OCR) requires an OCR engine connected through the [IOcrConnector](https://reference.groupdocs.com/search/net/groupdocs.search.options/iocrconnector/) interface; see [OCR support]({{< ref "search/net/developer-guide/advanced-usage/indexing/ocr-support.md" >}}).

## Supported Frameworks

The library is built for two target frameworks:

*   **.NET Framework 4.6.2** — the assembly can be used in applications for .NET Framework 4.6.2, 4.7, 4.7.1, 4.7.2, 4.8 and 4.8.1.
*   **.NET Standard 2.1** — the assembly can be used in applications for .NET Core 3.0, .NET Core 3.1, .NET 5, .NET 6, .NET 7, .NET 8, .NET 9 and .NET 10.

## Supported Operating Systems

GroupDocs.Search for .NET supports 32-bit (x86) and 64-bit (x64) operating systems on which a supported framework can be installed, including, but not limited to:

### Windows

*   Microsoft Windows 7 SP1
*   Microsoft Windows 8.1
*   Microsoft Windows 10
*   Microsoft Windows 11
*   Microsoft Windows Server 2008 R2 SP1
*   Microsoft Windows Server 2012, 2012 R2
*   Microsoft Windows Server 2016
*   Microsoft Windows Server 2019
*   Microsoft Windows Server 2022
*   Microsoft Windows Server 2025

The operating system must also be supported by the framework version used by the application. For example, .NET 8 and later versions do not support Windows 7, Windows 8.1 and Windows Server 2008 R2.

### Linux

*   Linux distributions supported by .NET Core 3.x or .NET 5 and later (Ubuntu, Debian, openSUSE, CentOS, RHEL and others)

Some formats are not supported on Linux; see [Known issues]({{< ref "search/net/developer-guide/known-issues.md" >}}).

## Development Environments

GroupDocs.Search for .NET can be used to develop applications in any development environment that targets a supported framework, for example:

*   Microsoft Visual Studio 2017 or later — for applications that target .NET Framework 4.6.2 or later.
*   Microsoft Visual Studio 2019 version 16.3 or later — for applications that target .NET Core 3.0 or later. Newer .NET versions require newer versions of Visual Studio: for example, .NET 6 and later require Visual Studio 2022.
*   Visual Studio Code, JetBrains Rider or the .NET SDK command-line tools of a version that supports the target framework.
