---
id: mcp-tool-get-text-fragments
url: search/mcp/tools-reference/get-text-fragments
title: get_text_fragments
weight: 3
description: "The get_text_fragments MCP tool returns the passages of indexed documents that match a query, with surrounding words, so an AI agent can quote a clause without reading the whole file."
keywords: get_text_fragments MCP tool, quote passage from document AI, matching text fragments, grounded answers from documents
productName: GroupDocs.Search MCP Server
generated: true
serverVersion: 26.10.1
toc: True
---

`get_text_fragments` returns **the passages that match**, each with a few words of context, grouped per document — the call an agent makes to quote a clause instead of reading a whole file. It takes the same query language as [`search`]({{< ref "search/mcp/tools-reference/search.md" >}}). Example prompt: *"What do the contracts say about the termination penalty? Quote it."*

**Tool description (as the AI agent sees it):**

> Returns the passages of indexed documents that match a query — each match with the words around it — so the user can read what a document says without opening it. Call this when the user wants to see the matching text, quote a clause, or answer a question from their documents ('what does the lease say about termination?'), usually after search has shown which files match. Takes the same query syntax as search. Works on documents indexed by this server (text is stored in the index). Returns plain text: a header line with the document count, then per document a '=== <file> (<n> occurrence(s)) ===' line followed by up to maxFragmentsPerDocument fragments, each prefixed with the field it came from. In evaluation mode at most 10 documents are considered. On failure the response starts with 'Fragment extraction failed for' followed by the reason, and isError is set.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `query` | string | yes | Search query, same syntax as the search tool. |
| `termsBefore` | integer | no | Words of context before each match. Default 8. |
| `termsAfter` | integer | no | Words of context after each match. Default 8. |
| `maxFragmentsPerDocument` | integer | no | Maximum fragments per document. Default 3. |
| `maxDocuments` | integer | no | Maximum documents to include. Default 5. |
| `indexName` | string | no | Name of the index, e.g. 'contracts'. Omit to use the default index. |

## Example call

```json
{
  "name": "get_text_fragments",
  "arguments": {
    "query": "\"termination penalty\"",
    "indexName": "contracts"
  }
}
```

## Result

Plain text. Captured from server 26.10.1 (evaluation note trimmed):

```text
Query '"termination penalty"' in index 'contracts': 2 document(s) match.

=== contracts/acme-master-services-agreement.pdf (1 occurrence(s)) ===
  [content] …terminates before the end of the Term, a termination penalty of
three monthly fees applies, payable within 30…

=== contracts/globex-consulting-agreement.docx (1 occurrence(s)) ===
  [content] …at any time with 30 days notice.
Early termination penalty: the Client pays 50 percent of the fees…
```

* `termsBefore` / `termsAfter` (default 8 each) set how many **words** of context surround a match — raise them to 20 for a full clause.
* `maxFragmentsPerDocument` (default 3) and `maxDocuments` (default 5) cap the size of the answer.
* Each fragment is prefixed with the field it came from — usually `[content]`. A match in a file name or a file date appears under that field instead.
* A match inside an email attachment shows as `=== mail/message.eml → attachment.docx (…) ===`.

Context is counted in words, not sentences, so a fragment can start mid-sentence. On failure the response starts with `Fragment extraction failed for` and `isError` is set.

## Example prompts

* *"Quote the termination penalty clause from each contract."*
* *"Show me where the datasheet mentions the operating temperature."*
* *"What does the quotation in my mail say about the price increase?"*

See it used end-to-end: [Quote matching passages, not whole files]({{< ref "search/mcp/use-cases/quote-matching-passages-not-whole-files.md" >}}).
