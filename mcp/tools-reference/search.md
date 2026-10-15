---
id: mcp-tool-search
url: search/mcp/tools-reference/search
title: search
weight: 2
description: "The search MCP tool finds the documents in an index that match a query — words, quoted phrases, AND/OR/NOT, fuzzy, wildcards, date and number ranges — and returns them as JSON with relevance and occurrences."
keywords: search MCP tool, full-text search AI agent, which documents mention, boolean phrase search MCP, date range search documents
productName: GroupDocs.Search MCP Server
generated: true
serverVersion: 26.10.1
toc: True
---

`search` answers **which documents** match a query and returns them as JSON — the file, a relevance score, how many times the terms occur, and which terms were found. Example prompt: *"Which contracts mention a termination penalty?"*

**Tool description (as the AI agent sees it):**

> Searches an index for documents that contain the query and returns the matching files ranked by relevance, with occurrence counts and the terms found. Call this whenever the user asks which documents mention something, to find, look up or search content across their files — after the documents have been indexed with add_to_index. Query syntax (GroupDocs.Search DSL): a single word ('invoice'); a phrase in double quotes ('"force majeure"') — always quote multi-word phrases, unquoted words separated by spaces do not match; combine with AND or &, OR or |, NOT or !, and parentheses ('termination AND (notice OR penalty)'); '?' replaces one character ('?ffect', 'princip?(2~4)'); date ranges daterange(2026-01-01 ~~ 2026-06-30); number ranges 100 ~~ 500; field prefixes filename: or content: ('filename: invoice'); a query starting with ^ is a regular expression ('^inv[0-9]+'). There is no trailing '*' wildcard — use ? patterns or fuzzy=true for spelling variants. To read the matching paragraphs rather than just the file names, call get_text_fragments with the same query. Returns JSON: `query`, `index`, `documentsFound`, `documentsReturned`, `totalOccurrences`, `truncated`, optional `evaluation`, and `documents` (array of { `filePath` relative to the storage folder, `innerPath` for items inside archives or emails, `relevance`, `occurrences`, `terms`, `fields` }). documentsFound 0 means nothing matched — not an error. In evaluation mode at most 10 documents are returned. On failure the response starts with 'Search failed for' followed by the reason, and isError is set; an unknown index lists the indexes that exist.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `query` | string | yes | Search query, e.g. 'invoice', 'termination AND notice', '"force majeure"' (quoted phrase), 'filename: lease', 'daterange(2026-01-01 ~~ 2026-12-31)'. |
| `maxResults` | integer | no | Maximum number of documents to return. Default 10. |
| `fuzzy` | boolean | no | Also match words with small spelling differences (fuzzy search). Default false. |
| `caseSensitive` | boolean | no | Match letter case exactly. Default false. |
| `indexName` | string | no | Name of the index to search, e.g. 'contracts'. Omit to use the default index. |

## Example call

```json
{
  "name": "search",
  "arguments": {
    "query": "termination AND penalty",
    "indexName": "contracts"
  }
}
```

## Result

JSON. Captured from server 26.10.1 (the `evaluation` note and the per-field `fields` arrays are trimmed):

```json
{
  "query": "termination AND penalty",
  "index": "contracts",
  "documentsFound": 3,
  "documentsReturned": 3,
  "totalOccurrences": 9,
  "truncated": false,
  "documents": [
    { "filePath": "contracts/acme-master-services-agreement.pdf",
      "relevance": 0.023, "occurrences": 3,
      "terms": [ "termination", "penalty" ] },
    { "filePath": "contracts/globex-consulting-agreement.docx",
      "relevance": 0.038, "occurrences": 4,
      "terms": [ "termination", "penalty" ] },
    { "filePath": "contracts/initech-nda.docx",
      "relevance": 0.023, "occurrences": 2,
      "terms": [ "termination", "penalty" ] }
  ]
}
```

| Field | Meaning |
|---|---|
| `documentsFound` / `documentsReturned` | matches in the index / documents in this response (`maxResults`, default 10) |
| `truncated` | `true` when more documents matched than were returned |
| `documents[].filePath` | relative to the storage folder |
| `documents[].innerPath` | the attachment or archive member, when the match is inside an email or archive |
| `relevance`, `occurrences`, `terms`, `fields` | the score, the hit count, the words found, and per-field counts |
| `hint` | present on an empty result when the query looks like an unquoted phrase |
| `evaluation` | present while evaluation limits apply |

Documents are returned in the engine's order — sort by `relevance` if order matters to you. `documentsFound: 0` is an answer, not an error.

An unquoted two-word query is the most common miss. The server explains it:

```json
{ "query": "termination penalty", "documentsFound": 0,
  "hint": "Unquoted words separated by spaces do not match. ..." }
```

## Query language

| You want | Query | Note |
|---|---|---|
| a word | `penalty` | case-insensitive unless `caseSensitive: true` |
| a phrase | `"termination penalty"` | **always quote multi-word phrases** — unquoted words do not match |
| all of / any of / not | `termination AND penalty` · `renewal OR renews` · `lease NOT draft` | also `&`, `\|`, `!`; group with parentheses |
| one unknown character | `terminat?` | matches `terminate`, not `termination` — `?` is one character |
| spelling variants | `renewl` with `fuzzy: true` | also matches near words — check the `terms` it found |
| dates written in documents | `content: daterange(2026-07-01 ~~ 2026-09-30)` | **use `content:`** — see the note below |
| numbers in a range | `content: (100000 ~~ 999999)` | whole numbers written without separators |
| a file-name word | `filename: q3` | matches words in the file name |
| a regular expression | `^inv[0-9]+` | a query starting with `^` |

There is **no trailing `*` wildcard**: use `?` patterns or `fuzzy: true` for variants. `AND` is evaluated per **document**, not per sentence — confirm with [`get_text_fragments`]({{< ref "search/mcp/tools-reference/get-text-fragments.md" >}}) that the words are about the same thing.

{{< alert style="warning" >}}
**Put `content:` in front of a date range.** The engine also indexes each file's creation and modification dates as fields, so a bare `daterange(…)` matches every file written in that period. `content: daterange(…)` searches only the dates written inside the documents. To search by file date on purpose, use `modificationdate: daterange(…)`.
{{< /alert >}}

On failure the response starts with `Search failed for` and `isError` is set; an unknown index lists the indexes that exist.

## Example prompts

* *"Which contracts mention a termination penalty?"*
* *"Find the documents that say "force majeure"."*
* *"Which reports mention a date in Q3 2026?"*
* *"Search the mail index for invoice 1042."*

See it used end-to-end: [Search documents with AI agents]({{< ref "search/mcp/use-cases/search-documents-with-ai-agents.md" >}}).
