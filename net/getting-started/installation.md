---
id: installation
url: search/net/installation
title: Installation
weight: 5
description: "This article describes how to install GroupDocs.Search for .NET from NuGet or from the official GroupDocs website."
keywords: install, NuGet, GroupDocs.Search, runtime packages, GroupDocs.Search.NETFramework, Package Manager, .NET CLI
productName: GroupDocs.Search for .NET
hideChildren: False
---

## Install from Nuget

NuGet is the easiest way to download and install GroupDocs.Search for .NET. Install the **GroupDocs.Search** package in projects of all [supported frameworks]({{< ref "search/net/getting-started/system-requirements.md" >}}), including .NET Framework: NuGet installs with it the runtime package that contains the build of the library for the target framework of the project (see [NuGet packages](#nuget-packages)). There are three ways to install it in your project.

#### Install via Package Manager GUI

Follow these steps to reference GroupDocs.Search using Package Manager GUI:

*   Open your solution/project in Visual Studio.
*   Click Tools -> NuGet Package Manager -> Manage NuGet Packages for Solution.  
    You can also access the same option through the Solution Explorer. Right-click the solution or project and select Manage NuGet Packages from the context menu.
*   Select Browse tab and type 'GroupDocs.Search' in the search text box.
*   Click the Install button to install the latest version of the API into your project as shown in the following screenshot.

![](/search/net/images/installation.png)

#### Using Package Manager Console

You can follow the steps below to reference GroupDocs.Search for .NET using the Package Manager Console:

*   Open your solution/project in Visual Studio.
*   Select Tools -> NuGet Package Manager -> Package Manager Console from the menu to open package manager console.
*   Type the command 'Install-Package GroupDocs.Search' and press enter to install the latest release into your application.  
    After successful installation, GroupDocs.Search will be referenced in your application.  

![](/search/net/images/installation_1.png)

```
PM> NuGet\Install-Package GroupDocs.Search
```

#### Using .NET CLI

Run the following command in the folder of the project:

```
dotnet add package GroupDocs.Search
```

#### NuGet packages

Since version 26.11, GroupDocs.Search for .NET is published on NuGet as the [GroupDocs.Search](https://www.nuget.org/packages/GroupDocs.Search) package and six runtime packages. GroupDocs.Search has no assemblies of its own: for each supported target framework, it depends on the runtime package of the same version that contains the build of the library for that framework, and NuGet installs only the runtime package that matches the target framework of the project.

| Runtime package | Build of the library | Installed for projects that target |
| --- | --- | --- |
| [GroupDocs.Search.Net462](https://www.nuget.org/packages/GroupDocs.Search.Net462) | .NET Framework 4.6.2 | .NET Framework 4.6.2, 4.7, 4.7.1 and 4.7.2 |
| [GroupDocs.Search.Net48](https://www.nuget.org/packages/GroupDocs.Search.Net48) | .NET Framework 4.8 | .NET Framework 4.8 and 4.8.1 |
| [GroupDocs.Search.NetStandard21](https://www.nuget.org/packages/GroupDocs.Search.NetStandard21) | .NET Standard 2.1 | .NET Core 3.0 and 3.1, .NET 5 and .NET Standard 2.1 |
| [GroupDocs.Search.Net60](https://www.nuget.org/packages/GroupDocs.Search.Net60) | .NET 6 | .NET 6 and .NET 7 |
| [GroupDocs.Search.Net80](https://www.nuget.org/packages/GroupDocs.Search.Net80) | .NET 8 | .NET 8 and .NET 9 |
| [GroupDocs.Search.Net100](https://www.nuget.org/packages/GroupDocs.Search.Net100) | .NET 10 | .NET 10 and later |

The API of the library is the same in all runtime packages. A runtime package can also be installed directly instead of the GroupDocs.Search package.

#### Packages of the AI search

Since version 26.12, one more package provides an optional part of the [AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/_index.md" >}}): it runs the neural networks of the models. The GroupDocs.Search package does not depend on it: an application that does not use the neural models does not need it.

| Package | What it provides | Dependencies |
| --- | --- | --- |
| [GroupDocs.Search.AI.Onnx](https://www.nuget.org/packages/GroupDocs.Search.AI.Onnx) | Runs the neural networks of the models of the search by meaning by ONNX Runtime. | GroupDocs.Search; Microsoft.ML.OnnxRuntime.Managed 1.22.1 or later |

The package has the builds for the same target frameworks as the library, and its version is the version of the library.

The GroupDocs.Search.AI.Onnx package references only the managed part of ONNX Runtime. Add the package with the native library of ONNX Runtime for your device to the application: Microsoft.ML.OnnxRuntime for a processor, Microsoft.ML.OnnxRuntime.Gpu for a video card of NVIDIA, or Microsoft.ML.OnnxRuntime.DirectML for DirectML on Windows; see [Models for AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md#onnx-runtime-adapter" >}}).

```
dotnet add package GroupDocs.Search.AI.Onnx
dotnet add package Microsoft.ML.OnnxRuntime
```

The language models need no package: the services with the chat completions API, such as OpenAI, Azure OpenAI, Ollama and LM Studio, are connected by the GroupDocs.Search package itself; see [Language models]({{< ref "search/net/developer-guide/advanced-usage/ai-search/language-models.md" >}}).

The models of the search by meaning are not NuGet packages: they are folders of files that are prepared separately, see [Preparing model packages]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md#preparing-model-packages" >}}).

#### Upgrading from GroupDocs.Search.NETFramework

From version 24.12 to version 26.10, the GroupDocs.Search package contained only the build for .NET Standard 2.1, and .NET Framework projects used the **GroupDocs.Search.NETFramework** package, which has no versions after 26.10. To upgrade a .NET Framework project to version 26.11 or later, uninstall GroupDocs.Search.NETFramework and install GroupDocs.Search. Both packages contain the GroupDocs.Search assembly, so a project must not reference both of them.

```
PM> NuGet\Uninstall-Package GroupDocs.Search.NETFramework
PM> NuGet\Install-Package GroupDocs.Search
```

## Install from official GroupDocs website

You can follow the steps below to reference GroupDocs.Search for .NET downloaded from official website [Downloads section](https://downloads.groupdocs.com/search/net):

1.  Unpack zip archive or follow MSI install wizard instructions.
2.  In the Solution Explorer, expand the project node you want to add a reference to.
3.  Right-click the **References** node for the project and select **Add Reference** from the menu.
4.  In the Add Reference dialog box, select the **.NET** tab (it is usually selected by default).
5.  If you have used MSI installer to install GroupDocs.Search, you will see GroupDocs.Search in the top pane. Select it and then click the **Select** button.
6.  If you have downloaded and unpacked the DLL only, click the **Browse** button and locate the GroupDocs.Search.dll file of the target framework of your project: the archive contains a build of the library for each supported target framework (see [System requirements]({{< ref "search/net/getting-started/system-requirements.md" >}})). The readme.txt file of the archive lists the NuGet packages that each build depends on; add them to the project as well.   
    You have referenced GroupDocs.Search and it should appear in the **SelectedComponents** pane of the dialog box.
7.  Click **OK**.  
    GroupDocs.Search reference appears under the **References** node of the project.
