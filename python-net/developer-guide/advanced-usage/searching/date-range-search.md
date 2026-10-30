---
id: date-range-search
url: search/python-net/date-range-search
title: Date range search
weight: 3
description: "This article describes how to search by date with date range search and how to specify date formats."
keywords: search by date, date range search
productName: GroupDocs.Search for Python via .NET
hideChildren: False
---
This page describes how you can search by date with date range search. It also describes the creation of date range search queries and the management of formats for date range search.

## Creating date range search queries

Date range search allows you to search documents for dates from a given range written in given date formats.

Queries for date range search in text form are specified in the following format:

*   daterange( date ~~ date )

where date is the date in yyyy-MM-dd format, for example, 2019-09-13. Please note that in a search query, dates are always set in the same format.

The following example demonstrates how to search by date using queries in text and object form.

{{< tabs "example1" >}}
{{< tab "Python" >}}
```python
import datetime
import os

from groupdocs.search import Index, SearchQuery

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Search for dates using query in text form
query1 = "daterange(2017-01-01 ~~ 2019-12-31)"
result1 = index.search(query1)

# Search for dates using query in object form
query2 = SearchQuery.create_date_range_query(datetime.datetime(2017, 1, 1), datetime.datetime(2019, 12, 31))
result2 = index.search(query2)

# Printing the documents found
print("Documents found by the text query:", result1.document_count)
print("Documents found by the object query:", result2.document_count)
for document in result2:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/catalogue-us.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/catalogue-us.txt), [documents/observatory-log.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/observatory-log.txt), [documents/seminar-programme.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/seminar-programme.txt), [documents/ausgabe-de.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/ausgabe-de.txt), [documents/archive-note.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/archive-note.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "creating-date-range-search-queries.txt" >}}  
```text
Documents found by the text query: 3
Documents found by the object query: 3
    catalogue-us.txt
    observatory-log.txt
    seminar-programme.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/date-range-search/creating_date_range_search_queries/creating-date-range-search-queries.txt)
{{< /tab >}}
{{< /tabs >}}

## Specifying date range search formats

To perform the date range search, the date templates specified in the search options are used. The following formats are used by default:

*   dd.MM.yyyy
*   MM/dd/yyyy
*   yyyy-MM-dd

Formats for date range search are set in the [date_formats](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/date_formats/) property of the [SearchOptions](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/searchoptions/) class. To add a format, use the [add](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatcollection/add/) method, to remove it, use the [remove](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatcollection/remove/) method, to clear a collection of existing formats, use the [clear](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatcollection/clear/) method. Each format in the collection is defined by an instance of the [DateFormat](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformat/) class. To create a new format, you must specify a sequence of format elements and a date separator string. The sequence must contain at least two elements. All available format elements are represented in the [DateFormatElement](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/) class:

*   [DAY_OF_MONTH](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/day_of_month/) specifies the day of month element, represented by one or two digits;
*   [DAY_OF_MONTH_TWO_DIGITS](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/day_of_month_two_digits/) specifies the day of month element, represented by two digits;
*   [MONTH](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/month/) specifies the month element, represented by one or two digits;
*   [MONTH_TWO_DIGITS](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/month_two_digits/) specifies the month element, represented by two digits;
*   [MONTH_ABBREVIATED_NAME](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/month_abbreviated_name/) specifies the month element, represented by abbreviated name;
*   [MONTH_FULL_NAME](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/month_full_name/) specifies the month element, represented by full name;
*   [YEAR](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/year/) specifies the year element, represented by one, two, three, or four digits;
*   [YEAR_TWO_DIGITS](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/year_two_digits/) specifies the year element, represented by two digits;
*   [YEAR_FOUR_DIGITS](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/year_four_digits/) specifies the year element, represented by four digits;
*   [DATE_SEPARATOR](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/date_separator/) specifies the date separator element.

An example of setting the date format for the search is presented below.

{{< tabs "example2" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import DateFormat, DateFormatElement, SearchOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Setting date formats
options = SearchOptions()
options.date_formats.clear()  # Removing default date formats
elements = [
    DateFormatElement.MONTH_TWO_DIGITS,
    DateFormatElement.DATE_SEPARATOR,
    DateFormatElement.DAY_OF_MONTH_TWO_DIGITS,
    DateFormatElement.DATE_SEPARATOR,
    DateFormatElement.YEAR_FOUR_DIGITS,
]
# Creating a date format pattern 'MM/dd/yyyy'
date_format = DateFormat(elements, "/")
options.date_formats.add(date_format)

# Searching in the index.
# For the given query, for example, the date 09/27/2019 will be found,
# but the date 2019-09-27 will not be found, because it is presented in a format that is not specified in the search options.
result = index.search("daterange(2017-01-01 ~~ 2019-12-31)", options)

# Printing the documents found
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/catalogue-us.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/catalogue-us.txt), [documents/observatory-log.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/observatory-log.txt), [documents/seminar-programme.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/seminar-programme.txt), [documents/ausgabe-de.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/ausgabe-de.txt), [documents/archive-note.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/archive-note.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "specifying-date-range-search-formats.txt" >}}  
```text
Documents found: 1
    catalogue-us.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/date-range-search/specifying_date_range_search_formats/specifying-date-range-search-formats.txt)
{{< /tab >}}
{{< /tabs >}}

The [DateFormat](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformat/) class has two constructors:

*   DateFormat(format_elements, date_separator) creates a format with the specified date separator; format_elements is a list of [DateFormatElement](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/) values. Month names, if the format contains them, are taken from the current culture.
*   DateFormat(culture_name, format_elements) creates a format that uses the date separator and the month names of the specified culture, for example, "en-US" or "de-DE". This constructor is useful for formats containing the [MONTH_ABBREVIATED_NAME](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/month_abbreviated_name/) or [MONTH_FULL_NAME](https://reference.groupdocs.com/search/python-net/groupdocs.search.options/dateformatelement/month_full_name/) elements.

The following example demonstrates the creation of a format with month names in German, for example, '27.September.2019'.

{{< tabs "example3" >}}
{{< tab "Python" >}}
```python
import os

from groupdocs.search import Index
from groupdocs.search.options import DateFormat, DateFormatElement, SearchOptions

# Creating an index in the specified folder
index = Index("./index")

# Indexing documents from the specified folder
index.add("./documents")

# Creating a date format pattern 'dd.MMMM.yyyy' with the date separator and month names of the German culture
elements = [
    DateFormatElement.DAY_OF_MONTH_TWO_DIGITS,
    DateFormatElement.DATE_SEPARATOR,
    DateFormatElement.MONTH_FULL_NAME,
    DateFormatElement.DATE_SEPARATOR,
    DateFormatElement.YEAR_FOUR_DIGITS,
]
date_format = DateFormat("de-DE", elements)

# Adding the format to the default date formats
options = SearchOptions()
options.date_formats.add(date_format)
print("Date formats:", ", ".join(f.to_string() for f in options.date_formats))

# Searching in the index
result = index.search("daterange(2017-01-01 ~~ 2019-12-31)", options)

# Printing the documents found
print("Documents found:", result.document_count)
for document in result:
    print("   ", os.path.basename(document.document_info.file_path))
```
{{< /tab >}}
{{< tab "documents" >}}
{{< tab-text >}}
The following sample files are used in this example: [documents/catalogue-us.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/catalogue-us.txt), [documents/observatory-log.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/observatory-log.txt), [documents/seminar-programme.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/seminar-programme.txt), [documents/ausgabe-de.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/ausgabe-de.txt), [documents/archive-note.txt](/search/python-net/_sample_files/developer-guide/advanced-usage/searching/date-range-search/documents/archive-note.txt)
{{< /tab-text >}}
{{< /tab >}}
{{< tab "specifying-date-range-search-formats-2.txt" >}}  
```text
Date formats: dd\.MM\.yyyy, MM\/dd\/yyyy, yyyy\-MM\-dd, dd/MMMM/yyyy
Documents found: 3
    catalogue-us.txt
    observatory-log.txt
    seminar-programme.txt
```
[Download full output](/search/python-net/_output_files/developer-guide/advanced-usage/searching/date-range-search/specifying_date_range_search_formats_2/specifying-date-range-search-formats-2.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for Python via .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET)

### Free online document search App

Along with the full-featured library, we provide simple but powerful free apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
