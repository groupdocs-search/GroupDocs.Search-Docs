---
id: date-range-search
url: search/java/date-range-search
title: Date range search
weight: 3
description: ""
keywords: 
productName: GroupDocs.Search for Java
hideChildren: False
---
This page describes how you can search by date with date range search. As well it describes the creation of date wise search queries, and also describes the management of formats for date range search.

## Creating date range search queries

Date range search allows you to search in documents dates from a given range in given date formats.

Queries for date range search in text form are specified in the following format:

*   daterange( date ~~ date )

where date is the date in yyyy-MM-dd format, for example, 2019-09-13. Please note that in a search query, dates are always set in the same format.

The following example demonstrates how to search by date using queries in text and object form.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Search for dates using query in text form
String query1 = "daterange(2017-01-01 ~~ 2019-12-31)";
SearchResult result1 = index.search(query1);
 
// Search for dates using query in text form
SearchQuery query2 = SearchQuery.createDateRangeQuery(new Date(2017 - 1900, 1 - 1, 1), new Date(2019 - 1900, 12 - 1, 31));
SearchResult result2 = index.search(query2);
```

## Specifying date range search formats

To perform the date range search, the date templates specified in the search options are used. The following formats are used by default:

*   dd.MM.yyyy
*   MM/dd/yyyy
*   yyyy-MM-dd

Formats for date range search are set in the collection that is returned by the [getDateFormats](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/SearchOptions#getDateFormats()) method of the [SearchOptions](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/SearchOptions) class. To add a format, use the [addItem](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatCollection#addItem(com.groupdocs.search.options.DateFormat)) method, to remove it, use the [removeItem](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatCollection#removeItem(com.groupdocs.search.options.DateFormat)) method, to clear a collection of existing formats, use the [clear](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatCollection#clear()) method. Each format in the collection is defined by an instance of the [DateFormat](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormat) class. To create a new format, you must specify a sequence of format elements and a separator character. All available format elements are represented in the [DateFormatElement](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement) class:

*   [getDayOfMonth](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getDayOfMonth()) method returns the day of month element, represented by one or two digits;
*   [getDayOfMonthTwoDigits](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getDayOfMonthTwoDigits()) method returns the day of month element, represented by two digits;
*   [getMonth](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getMonth()) method returns the month element, represented by one or two digits;
*   [getMonthTwoDigits](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getMonthTwoDigits()) method returns the month element, represented by two digits;
*   [getMonthAbbreviatedName](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getMonthAbbreviatedName()) method returns the month element, represented by abbreviated name;
*   [getMonthFullName](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getMonthFullName()) method returns the month element, represented by full name;
*   [getYear](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getYear()) method returns the year element, represented by one, two, three, or four digits;
*   [getYearTwoDigits](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getYearTwoDigits()) method returns the year element, represented by two digits;
*   [getYearFourDigits](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getYearFourDigits()) method returns the year element, represented by four digits;
*   [getDateSeparator](https://apireference.groupdocs.com/search/java/com.groupdocs.search.options/DateFormatElement#getDateSeparator()) method returns the date separator element.

An example of setting the date format for the search is presented below.



```java
String indexFolder = "c:\\MyIndex\\";
String documentsFolder = "c:\\MyDocuments\\";
 
// Creating an index in the specified folder
Index index = new Index(indexFolder);
 
// Indexing documents from the specified folder
index.add(documentsFolder);
 
// Setting date formats
SearchOptions options = new SearchOptions();
options.getDateFormats().clear(); // Removing default date formats
DateFormatElement[] elements = new DateFormatElement[]
{
    DateFormatElement.getMonthTwoDigits(),
    DateFormatElement.getDateSeparator(),
    DateFormatElement.getDayOfMonthTwoDigits(),
    DateFormatElement.getDateSeparator(),
    DateFormatElement.getYearFourDigits(),
};
// Creating a date format pattern 'MM/dd/yyyy'
DateFormat dateFormat = new DateFormat(elements, "/");
options.getDateFormats().addItem(dateFormat);
 
// Searching in the index.
// For the given query, for example, the date 09/27/2019 will be found,
// but the date 2019-09-27 will not be found, because it is presented in a format that is not specified in the search options.
SearchResult result = index.search("daterange(2017-01-01 ~~ 2019-12-31)", options);
```

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)
    
*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)
    

### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
