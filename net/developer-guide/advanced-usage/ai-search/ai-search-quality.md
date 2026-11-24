---
id: ai-search-quality
url: search/net/ai-search-quality
title: Quality and performance of AI search
weight: 13
description: "Measured quality and performance of the AI search of GroupDocs.Search for .NET: the ranked search within a language and between languages on the XQuAD set, the sorting of documents into categories without a language model, the speed of the encoding and of the search, and the size of the AI data."
keywords: search quality, recall, MRR, XQuAD, hybrid search benchmark, cross-language search, neural sparse, static embeddings, document classification accuracy, encoding speed
productName: GroupDocs.Search for .NET
hideChildren: False
---
This article shows what the AI search of GroupDocs.Search for .NET 26.12 gives in numbers: how much more exactly the [ranked search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md" >}}) finds with each model, how well the index sorts documents into categories without a language model, and what the functions cost in time and in disk space. All the numbers are measured on the library itself, through its public API, on public collections of texts.

Your documents and queries differ from the test collections, so use the numbers to compare the ways of the search with each other, not as a promise for your data.

## Results at a glance

| Question | Result |
| --- | --- |
| How often is the right document the first result, the question and the documents in one language? | The search by words: 80 to 92 percent. With the neural terms and the word vectors: 92 to 95 percent. |
| The same, among the first ten results? | 93 to 99 percent by words; more than 99 percent with the models. |
| How often is the right document among the first ten results when the question is in another language? | 88 to 99 percent with the word vectors model. The search by words does not find between languages. |
| How much does the smallest step give: a word vectors model of 55 MB without a neural network? | The first result is right in 88 to 93 percent of the questions instead of 80 to 92 percent. |
| How well are documents sorted into categories without a language model? | Of the decisions that the index accepts, 86 to 99 percent are right; the rest of the documents are left without a decision. |

## Ranked search

### Conditions

*   **Collection.** [XQuAD](https://github.com/google-deepmind/xquad): 240 paragraphs of Wikipedia and 1190 questions that people have written to them, professionally translated into 12 languages. The paragraphs of one language are the documents of an index; a question must find its paragraph. The measurement uses English, Russian and German.
*   **Measures.** R@1 is the share of the questions whose paragraph is the first result; R@10 is the share of the questions whose paragraph is among the first ten results; MRR@10 is the mean reciprocal rank of the paragraph within the first ten.
*   **Models.** The neural terms: opensearch-neural-sparse-encoding-multilingual-v1 with 8-bit weights. The vectors: the word vectors model static-similarity-mrl-multilingual-v1 with 512 components, see [Models for AI search]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ai-search-models.md" >}}).
*   **Method.** The documents are encoded by the [Encode](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/encode/) method and searched by the [Search](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/search/) method with different sets of [channels]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md#channels" >}}).

### Question and documents in one language

R@1 / R@10 / MRR@10:

| Channels | English | Russian | German |
| --- | --- | --- | --- |
| Text | 0.918 / 0.991 / 0.949 | 0.799 / 0.934 / 0.852 | 0.852 / 0.974 / 0.900 |
| Neural terms | 0.915 / 0.989 / 0.944 | 0.867 / 0.981 / 0.912 | 0.838 / 0.975 / 0.888 |
| Word vectors | 0.828 / 0.982 / 0.889 | 0.730 / 0.945 / 0.809 | 0.784 / 0.963 / 0.851 |
| Text and neural terms | 0.950 / 0.995 / 0.969 | 0.906 / 0.987 / 0.939 | 0.924 / 0.990 / 0.951 |
| Text and word vectors | 0.930 / 0.995 / 0.959 | 0.876 / 0.983 / 0.917 | 0.910 / 0.988 / 0.943 |
| Neural terms and word vectors | 0.939 / 0.997 / 0.961 | 0.895 / 0.991 / 0.934 | 0.888 / 0.989 / 0.928 |
| All three channels | 0.950 / 0.997 / 0.971 | 0.923 / 0.992 / 0.951 | 0.942 / 0.992 / 0.963 |

What the table tells:

*   All three channels together are the best set for every language. The number of the questions whose paragraph is not the first result falls 1.6 to 2.6 times: from 8 to 5 percent in English, from 20 to 8 percent in Russian, from 15 to 6 percent in German.
*   The gain is the greatest for Russian. The search by words looks for the exact forms of the words, and a question and a document often differ in the case and the number of a word.
*   The channels complement each other: every pair is better than each of its channels alone.
*   The word vectors model, which needs no neural network and no runtime, gives a large part of the gain: 0.799 to 0.876 in Russian.

### Question in one language, documents in another

R@1 / R@10; the first language is the language of the questions:

| Channels | English to Russian | English to German | Russian to English | Russian to German | German to English | German to Russian |
| --- | --- | --- | --- | --- | --- | --- |
| Neural terms | 0.285 / 0.552 | 0.612 / 0.865 | 0.152 / 0.222 | 0.216 / 0.357 | 0.488 / 0.724 | 0.217 / 0.403 |
| Word vectors | 0.651 / 0.939 | 0.734 / 0.965 | 0.597 / 0.910 | 0.565 / 0.868 | 0.682 / 0.949 | 0.534 / 0.858 |
| Text and word vectors | 0.675 / 0.943 | 0.766 / 0.973 | 0.623 / 0.918 | 0.584 / 0.875 | 0.744 / 0.961 | 0.566 / 0.874 |
| All three channels | 0.668 / 0.948 | 0.767 / 0.986 | 0.621 / 0.920 | 0.597 / 0.882 | 0.771 / 0.962 | 0.587 / 0.882 |

What the table tells:

*   The search between languages is the work of the passage vectors channel. The search by words finds only the documents that happen to have a name or a number of the question.
*   The neural terms hardly search between languages with different scripts: the model expands a passage mostly by the words of its own language.
*   The other channels do not spoil the result of the vectors, because the channels are merged [with weights]({{< ref "search/net/developer-guide/advanced-usage/ai-search/ranked-search.md#how-the-channels-are-merged" >}}): a channel that has found little of what is asked weighs little.

### What the measurement does not tell

*   The questions of the collection are written to the paragraphs and repeat their words, so the collection is easy for the search by words. For queries that are worded independently of the documents, the channels of meaning contribute more than here.
*   The collection is small: 240 documents for a language. The quality on a large collection, and on business documents such as contracts and reports, is not measured: there are no public collections of such documents with known answers.
*   The table has no neural model of vectors and no reranker. Measure them on your own documents and queries.

## Sorting documents into categories

The [sorting of documents into categories]({{< ref "search/net/developer-guide/advanced-usage/ai-search/document-categories.md" >}}) is measured on four public collections of texts with known categories, without a language model: by the index channels alone. A category is described as its name with a few typical words. The documents are encoded by the same two models as above.

| Collection | Documents, categories | Right decisions among all the documents | Share of the documents with the confidence 0.8 and more | Right among them |
| --- | --- | ---: | ---: | ---: |
| BBC News: articles in English | 1200, 5 | 0.945 | 0.68 | 0.990 |
| AG News: short news in English | 1200, 4 | 0.809 | 0.41 | 0.914 |
| 10kGNAD: articles in German, the descriptions of the categories in English | 1028, 9 | 0.662 | 0.25 | 0.866 |
| 20 Newsgroups: messages in English, 20 close categories | 1200, 20 | 0.639 | 0.39 | 0.863 |

A document that is left without a decision counts as a wrong one in the third column.

What the table tells:

*   The index channels sort the documents of clearly different subjects reliably and leave the rest without a decision: 86 to 99 percent of the accepted decisions are right.
*   Close categories and the documents between two subjects are the work of a language model or of a person. The [Enrich](https://reference.groupdocs.com/search/net/groupdocs.search.ai/indexai/enrich/) method gives the language model exactly these documents.
*   The two channels together are better than either of them: the better channel alone is 1 to 3 points behind, and which channel is the better one differs between the collections.

## Speed and size

| What | Result | Where it is measured |
| --- | --- | --- |
| Encoding by the neural terms model with 8-bit weights | About 14 passages of 128 tokens in a second; about 750 MB of memory | Intel Core i5-11500, 6 cores |
| Encoding by the neural terms model with full weights | About 9 passages of 128 tokens in a second; about 1450 MB of memory | The same computer |
| Encoding by the word vectors model | About 400 passages in 0.2 to 0.6 seconds | A virtual machine, 2 cores |
| Ranked search by all three channels | 5.8 ms for a query: the text 0.4 ms, the neural terms 2.8 ms, the vectors 1.3 ms | A virtual machine, 2 cores; an index of 240 documents |
| Sorting of 1200 documents into categories by the index channels | 0.06 to 0.09 seconds | A virtual machine, 2 cores |
| Choice of 200 words for the suggestion of synonyms and the count of their documents | 0.04 to 0.34 seconds | A virtual machine, 2 cores |
| AI data of the index | About 0.55 KB of the neural terms and 0.58 KB of a vector of 512 components for a passage | The index of the XQuAD collection |

Notes:

*   The time of the encoding is the time of the neural network. The reading of the stored text and the division into passages take a small part of it.
*   The time of a search is measured on a small collection. On a large collection it grows with the number of the passages; it is not measured.
*   On the paragraphs of Wikipedia, the AI data of the two channels takes about 2.3 times the size of the text of the documents.
*   The time of the functions of a language model is the time of the model: an answer takes one or two requests, the sorting takes a request for a document that the index channels have not sorted.

## What is guaranteed by design

Some properties of the functions of a language model do not depend on the model and need no measurement:

*   **Answers with citations.** Every statement of the text of an answer has a quote that is found in the text of a document.
*   **Translation of requests.** The returned query is parsed by the parser of the index and can be searched in it.
*   **Suggestion of synonyms.** Every suggested synonym is one word by the rules of the index, the pair is not in the dictionary yet, and at least one word of the pair is in the documents.
*   **Sorting into categories.** A decision of the index channels below the threshold of confidence is not accepted.

## More resources

### GitHub examples

You may easily run the code from documentation articles and see the features in action in our GitHub examples:

*   [GroupDocs.Search for .NET examples](https://github.com/groupdocs-search/GroupDocs.Search-for-.NET)

*   [GroupDocs.Search for Java examples](https://github.com/groupdocs-search/GroupDocs.Search-for-Java)


### Free online document search App

Along with full featured .NET library we provide simple, but powerful free Apps.

You are welcome to search over your PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX and more with our free online [Free Online Document Search App](https://products.groupdocs.app/search).
