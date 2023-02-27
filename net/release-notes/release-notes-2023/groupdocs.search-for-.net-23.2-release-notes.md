---
id: groupdocs-search-for-net-23-2-release-notes
url: search/net/groupdocs-search-for-net-23-2-release-notes
title: GroupDocs.Search for .NET 23.2 Release Notes
weight: 120
description: ""
keywords: 
productName: GroupDocs.Search for .NET
hideChildren: False
---

{{< alert style="info" >}}This page contains release notes for GroupDocs.Search for .NET 23.2{{< /alert >}}

## Major Features

There are the following features, enhancements, and fixes in this release:

- Numeric range searching does not work when document contains too long numbers

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2815 | Numeric range searching does not work when document contains too long numbers | Fix |

## Public API and Backward Incompatible Changes

### Numeric range searching does not work when document contains too long numbers

This fix eliminates an error in searching for a range of numbers in files containing numbers longer than the type Int64 may contain.

##### Public API changes

None.

##### Use cases

None.

