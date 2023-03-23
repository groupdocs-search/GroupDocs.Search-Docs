---
id: groupdocs-search-for-java-23-3-release-notes
url: search/java/groupdocs-search-for-java-23-3-release-notes
title: com.groupdocs.search for Java 23.3 Release Notes
weight: 110
description: ""
keywords: 
productName: com.groupdocs.search for Java
hideChildren: False
---

{{< alert style="info" >}}
## NOTE: Starting from this release of the GroupDocs.Search for Java will support version Java 8 and above.
{{< /alert >}}

{{< alert style="info" >}}This page contains release notes for com.groupdocs.search for Java 23.3{{< /alert >}}

## Major Features

There are the following features, enhancements, and fixes in this release:

- Numeric range searching does not work when document contains too long numbers

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| SEARCHNET-2815 | Numeric range searching does not work when document contains too long numbers | Fix |

## Public API and Backward Incompatible Changes

### Numeric range searching does not work when document contains too long numbers

This fix eliminates an error in searching for a range of numbers in files containing numbers longer than the type long may contain.

##### Public API changes

None.

##### Use cases

None.
