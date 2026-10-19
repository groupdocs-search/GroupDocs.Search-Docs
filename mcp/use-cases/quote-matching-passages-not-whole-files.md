---
id: mcp-uc-quote-matching-passages-not-whole-files
url: search/mcp/use-cases/quote-matching-passages-not-whole-files
title: How to get the matching passages instead of reading whole files
linkTitle: Quote matching passages
weight: 3
description: "Let an AI agent quote the passages that answer a question — with a few words of context — instead of reading whole documents into its context, locally over MCP."
keywords: AI quote from documents, grounded answers from files, read only relevant paragraphs, token budget documents agent, get_text_fragments
productName: GroupDocs.Search MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to get the matching passages instead of reading whole files"
        description: "Let an AI agent quote the passages that answer a question — with a few words of context — instead of reading whole documents into its context, locally over MCP."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Search MCP server with Docker and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "What do my contracts say about penalties? Quote the passages, with enough context to understand each one."
---

An agent that reads whole documents to answer a question fills its context with text it does not need. With the GroupDocs.Search MCP server running locally, it calls `get_text_fragments` and gets only the passages that match, with as many words of context as you choose — so the answer cites the passage and the context stays small:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-search/search-net-mcp:latest
```

> What do my contracts say about penalties? Quote the passages, with enough context to understand each one.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server, which ships as a Docker image only. Installation and client setup: [MCP server for .NET]({{< ref "search/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* An index built with [`add_to_index`]({{< ref "search/mcp/tools-reference/add-to-index.md" >}}) — see [search documents with AI agents]({{< ref "search/mcp/use-cases/search-documents-with-ai-agents.md" >}}).

## Why fragments

```text
Reading the files                        Searching the index
-------------------------------------    -------------------------------------
agent opens every document               search: which files match (JSON)
  context grows with the corpus          get_text_fragments: the passages
  long files crowd out the question        (~8 words either side, per file)
  repeated for every question            answer cites file + passage
                                         the index is reused next time
```

The index already holds the text, so the server can cut out the passage around each match. The agent never loads the rest of the file.

## The dial: termsBefore and termsAfter

Captured from server 26.10.1. The defaults — 8 words either side — give a phrase:

```text
-> get_text_fragments {"query":"penalty","indexName":"contracts"}
<- 3 document(s) match.
   === contracts/acme-master-services-agreement.pdf (1 occurrence(s)) ===
     [content] …before the end of the Term, a termination penalty of
   three monthly fees applies, payable within 30…
```

Twenty words either side give the clause:

```text
-> get_text_fragments {"query":"penalty","indexName":"contracts",
     "termsBefore":20,"termsAfter":20,"maxFragmentsPerDocument":1}
<- === contracts/acme-master-services-agreement.pdf (1 occurrence(s)) ===
     [content] …convenience on 90 days written notice.
   12.3 If the Client terminates before the end of the Term, a termination
   penalty of three monthly fees applies, payable within 30 days of the
   termination date.
   14.2 Force majeure: neither party is…
```

| Parameter | Default | Raise it when |
|---|---|---|
| `termsBefore` / `termsAfter` | 8 | you need the whole clause or sentence |
| `maxFragmentsPerDocument` | 3 | a word occurs many times and you want every use |
| `maxDocuments` | 5 | the answer spans more files |

## When the words differ

Full-text search matches words, so let the agent try the variants:

```text
-> search {"query":"renewal OR renews","indexName":"contracts"}
<- umbrella-office-lease.pdf — terms ["renewal", "renews"]

-> search {"query":"renewl","indexName":"contracts","fuzzy":true}
<- umbrella-office-lease.pdf — terms ["renewal", "renews", "rent"]
```

Fuzzy matching also found `rent`: check the `terms` a fuzzy search reports before trusting it.

## Expected result

Each answer arrives with the file and the quoted passage it rests on. When the quote does not support the answer, you can see it — which is the point.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| A fragment starts mid-sentence | context is counted in words, not sentences | raise `termsBefore` |
| *"(no stored text for this document…)"* | the document was indexed without stored text | re-add it with `add_to_index` |
| A fragment contains *"Evaluation only. Created with GroupDocs.Parser…"* | evaluation mode adds this line to the text of some formats | license the server, then rebuild the index |

## FAQ

**How much context does this save?** It depends on the corpus; the fragments for one question are a few hundred words, against the full text of every candidate file. The bigger the files, the bigger the difference.

**Can the agent still read a whole file?** Yes, with its own file tools. Fragments are for finding and quoting; read the whole file when the passage is not enough.

**Are the fragments exact?** They are the indexed text, with `…` marking where the window was cut. Line breaks follow the document.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
Answer questions about documents in ./documents from passages, not whole files:
get_text_fragments (raise termsBefore/termsAfter for full clauses), quote the
passage and its file. Try OR-lists or fuzzy=true when a query finds nothing.
```

## Honest limits

* **Evaluation mode:** an index holds at most 100 documents and a search returns at most 10 — the response says so in its `evaluation` field. [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}).
* **Full-text, not semantic:** it matches words, not meaning. Ask the agent to try variants (`OR`, `fuzzy: true`).
* **No OCR:** scans without a text layer are indexed by their file properties only.
* **Docker only:** there is no `dnx` channel for this product ([why]({{< ref "search/net/mcp/_index.md" >}})).

## Related

* [Search documents with AI agents]({{< ref "search/mcp/use-cases/search-documents-with-ai-agents.md" >}})
* [Search a folder of contracts]({{< ref "search/mcp/use-cases/search-a-folder-of-contracts.md" >}})
* [Keep an index up to date]({{< ref "search/mcp/use-cases/keep-an-index-up-to-date.md" >}})
* [Search documents in a code repository]({{< ref "search/mcp/use-cases/search-project-documents-from-claude-code.md" >}})
* [Search email archives]({{< ref "search/mcp/use-cases/search-email-archives-locally.md" >}})
* [Find documents by date and amount]({{< ref "search/mcp/use-cases/find-documents-by-date-and-amount.md" >}})
* [On-premise deployment]({{< ref "search/mcp/use-cases/on-premise-document-search.md" >}})
* [Tools reference]({{< ref "search/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "search/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "search/mcp/troubleshooting-faq.md" >}})
* Questions: [Search forum](https://forum.groupdocs.com/c/search/16)

## Read more

* [Grounded answers from your files: quote the passage, not the model's memory](https://blog.groupdocs.com/search/grounded-answers-quote-passages-local-index-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

