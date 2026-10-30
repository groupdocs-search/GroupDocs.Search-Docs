---
id: agents-and-llm-integration
url: search/python-net/agents-and-llm-integration
title: AI agents and LLM integration
linkTitle: Agents and LLMs
description: "GroupDocs.Search for Python via .NET is AI agent and LLM friendly: machine-readable documentation, MCP servers, AGENTS.md shipped inside the pip package, and a local full-text index that finds the passages an AI pipeline needs."
weight: 6
keywords: AI, LLM, agent, MCP, machine-readable, documentation, Claude, GPT, Copilot, AGENTS.md, RAG, retrieval, full-text search, document search
productName: GroupDocs.Search for Python via .NET
hideChildren: False
toc: True
---

GroupDocs.Search for Python via .NET is designed to work with AI agents, LLMs, and code generation tools. The library ships machine-readable documentation in several formats, including an `AGENTS.md` file inside the pip package itself, so that AI assistants can discover and use the API without manual guidance.

## Built into the package

The `groupdocs-search-net` pip package includes an `AGENTS.md` file at `groupdocs/search/AGENTS.md`. AI coding assistants that scan installed packages (such as Claude Code, Cursor, and GitHub Copilot) can read it to learn the imports, the index lifecycle, the query language, events, callbacks, licensing, platform requirements, and troubleshooting tips. Its full text is reproduced [below](#agentsmd-reference).

After installing the package, you can find it with:

```bash
pip show -f groupdocs-search-net | grep AGENTS
```

## MCP server

GroupDocs provides an [MCP (Model Context Protocol) server](https://docs.groupdocs.com/mcp) that lets LLMs query the documentation on demand instead of loading it all at once. This saves tokens and lets your AI assistant fetch only what it needs for the current task.

To connect your AI tool to the MCP server, add the GroupDocs endpoint to your MCP configuration:

{{< tabs "mcp-setup">}}
{{< tab "Claude Code / Claude Desktop" >}}
```json
// Claude Code:    ~/.claude/settings.json (or project .mcp.json)
// Claude Desktop: ~/Library/Application Support/Claude/claude_desktop_config.json
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "GitHub Copilot" >}}
```json
// .vscode/mcp.json in your project root
{
  "servers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "Cursor" >}}
```json
// .cursor/mcp.json in your project root
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "Generic MCP" >}}
```json
// Any MCP-compatible client
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< /tabs >}}

See [https://docs.groupdocs.com/mcp](https://docs.groupdocs.com/mcp) for full setup instructions and the list of available tools.

### Searching your documents from an agent

The [GroupDocs.Search MCP server]({{< ref "search/mcp/_index.md" >}}) is a separate server that does the work itself: it lets an AI agent index a folder of documents and search it by content, locally on your machine, then quote the matching passages. The server is the .NET build of GroupDocs.Search and ships as a Docker image, so it runs alongside a Python project without a .NET installation.

Use the documentation MCP server when an agent writes Python code against this library, and the search MCP server when the agent itself looks things up in your documents.

## Machine-readable documentation

Every documentation page is available as a plain Markdown file that AI tools can fetch and process directly:

| Resource | URL |
|---|---|
| Full documentation (single file) | `https://docs.groupdocs.com/search/python-net/llms-full.txt` |
| Full documentation (all products) | `https://docs.groupdocs.com/llms-full.txt` |
| Individual page (any page) | Append `.md` to the page URL |
| First search solution | `https://docs.groupdocs.com/search/python-net/build-your-first-search-solution.md` |

### How to use with AI tools

Point your AI assistant to the full documentation file for comprehensive context:

```
Fetch https://docs.groupdocs.com/search/python-net/llms-full.txt and use it
as a reference for GroupDocs.Search for Python via .NET API.
```

## Why GroupDocs.Search is a good building block for AI pipelines

An agent that answers questions about your documents first has to find the right ones, and then the right passages in them. GroupDocs.Search keeps a full-text index of a folder on your own disk and answers both questions, across PDF, Office, OpenDocument, email, eBook, archive and image files:

- **Retrieve for RAG**: find documents by keyword, phrase, wildcard, regular expression, boolean and field queries, with a relevance score for each.
- **Quote the evidence**: get the passages around each found word, with the found words marked, ready to place in a prompt.
- **Forgive the query**: fuzzy search tolerates typos and OCR noise, and synonym, homophone and word-form search find what the user meant.
- **Stay current**: `add()` and `update()` index only the documents that are new or changed, and the index persists between runs.
- **Read what is inside**: index the documents in ZIP archives and email attachments, and connect your own OCR engine for scans.

A typical retrieval step looks like this:

{{< tabs "example-1" >}}
{{< tab "Python" >}}
```python
import json
import os

from groupdocs.search import Index, IndexSettings
from groupdocs.search.highlighters import FragmentHighlighter
from groupdocs.search.options import Compression, HighlightOptions, OutputFormat, TextStorageSettings

# Keep the extracted text in the index, so passages can be quoted without the original files
settings = IndexSettings()
settings.text_storage_settings = TextStorageSettings(Compression.HIGH)

index = Index("./index", settings)
index.add("./documents")  # On later runs, only new and changed documents are indexed

result = index.search("termination AND penalty")

# Words of context around each found word, and how the found words are marked
options = HighlightOptions()
options.terms_before = 8
options.terms_after = 8
options.terms_total = 30
options.term_highlight_start_tag = "**"
options.term_highlight_end_tag = "**"

found = sorted(result, key=lambda document: document.relevance, reverse=True)
passages = []
for document in found[:5]:  # The five most relevant documents
    highlighter = FragmentHighlighter(OutputFormat.PLAIN_TEXT)
    index.highlight(document, highlighter, options)
    fragments = [fragment for container in highlighter.get_result() for fragment in container.get_fragments()]
    passages.append({
        "file": os.path.basename(document.document_info.file_path),
        "occurrences": document.occurrence_count,
        "passages": fragments,
    })

# Feed `passages` into an agent's context or a RAG prompt
print(json.dumps(passages, indent=2))
```
{{< /tab >}}
{{< /tabs >}}

For three short contracts, two of which mention a termination penalty, it prints (abridged):

```text
[
  {
    "file": "supply-contract.txt",
    "occurrences": 4,
    "passages": [
      " end this agreement with ninety days notice.\nEarly **termination** by the buyer carries a **penalty** of 12 percent of the remaining order value",
      ".\nThe **termination** **penalty** is payable within thirty days of the notice"
    ]
  },
  ...
]
```

For end-to-end examples, see the [Developer Guide]({{< ref "search/python-net/developer-guide/_index.md" >}}). Every code example has a runnable counterpart in the [examples repository](https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET).

{{< alert style="info" >}}
Without a license, an index holds at most 100 documents and only the first 10 found documents can be read. Set the `GROUPDOCS_LIC_PATH` environment variable before an agent runs these steps. See [Licensing and evaluation]({{< ref "search/python-net/getting-started/licensing-and-subscription.md" >}}).
{{< /alert >}}

## Pair with other GroupDocs products

For richer AI document pipelines, combine GroupDocs.Search with:

- [GroupDocs.Parser for Python via .NET](https://docs.groupdocs.com/parser/python-net/): extract the tables, metadata and structured data of the documents a search finds.
- [GroupDocs.Conversion for Python via .NET](https://docs.groupdocs.com/conversion/python-net/): convert legacy and exotic formats before indexing.
- [GroupDocs.Viewer for Python via .NET](https://docs.groupdocs.com/viewer/python-net/): render the pages a search finds to PNG or HTML for vision models and human review.

## See also

- [Build your first search solution]({{< ref "search/python-net/developer-guide/basic-usage/build-your-first-search-solution.md" >}}): create an index, add documents, search and highlight
- [Developer Guide]({{< ref "search/python-net/developer-guide/_index.md" >}}): runnable examples for every search feature
- [API Reference](https://reference.groupdocs.com/search/python-net): full class and method documentation

## AGENTS.md reference

The complete `AGENTS.md` shipped in the 26.10 package:

````markdown
# GroupDocs.Search for Python via .NET -- AGENTS.md

> Instructions for AI agents working with this package.

Full-text search and indexing -- build a persistent index over a folder of documents and run keyword, phrase, wildcard, regular-expression, fuzzy, boolean, faceted, synonym, and homophone queries against it. Works across Word, Excel, PowerPoint, PDF, email, eBook, web, archive and plain-text formats through one unified API, with no MS Office or external software installed.

## Install

```bash
pip install groupdocs-search-net
```

Each release's wheels are also on https://releases.groupdocs.com/search/python-net/ for offline installs (`pip install <wheel file>`).

**Python**: 3.5 - 3.14 | **Platforms**: Windows x64, Linux x64 (glibc 2.27+), macOS 13+ (Intel and Apple Silicon)

## Resources

| Resource | URL |
|---|---|
| Documentation | https://docs.groupdocs.com/search/python-net/ |
| LLM-optimized docs | https://docs.groupdocs.com/search/python-net/llms-full.txt |
| API reference | https://reference.groupdocs.com/search/python-net/ |
| Code examples | https://github.com/groupdocs-search/GroupDocs.Search-for-Python-via-.NET |
| Release notes | https://releases.groupdocs.com/search/python-net/release-notes/ |
| PyPI | https://pypi.org/project/groupdocs-search-net/ |
| Free support forum | https://forum.groupdocs.com/c/search/ |
| Temporary license | https://purchase.groupdocs.com/temporary-license |

## MCP Server

If your environment has MCP configured, you can connect your AI tool to the GroupDocs documentation server for on-demand API lookups:

```json
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```

Works with Claude Code (`~/.claude/settings.json`), Cursor (`.cursor/mcp.json`), VS Code Copilot (`.vscode/mcp.json`), and any MCP-compatible client. If MCP is unavailable, fall back to the LLM-optimized docs URL above and this file -- both are shipped inside the wheel.

## Imports

```python
from groupdocs.search import (
    Index, IndexSettings, IndexRepository, IndexUpdater,
    Extractor, ExtractionHost, SearchQuery,
    License, Metered, LicenseError,
)
from groupdocs.search.options import (
    SearchOptions, FuzzySearchOptions, SimilarityLevel, TableDiscreteFunction,
    IndexingOptions, UpdateOptions, MergeOptions, OptimizeOptions, DeleteOptions,
    HighlightOptions, OutputFormat, IndexType, CommonFieldNames,
    SpellingCorrectorOptions, ImageSearchOptions,
)
from groupdocs.search.results import (
    SearchResult, FoundDocument, FoundDocumentField, DocumentInfo, FileType, DeleteResult,
    FormatFamily,
)
from groupdocs.search.common import (
    Document, DocumentField, ExtractedItemInfo, IndexInfo, IndexStatus, IndexingReport,
    SearchReport, OutputAdapter, FileOutputAdapter, StringOutputAdapter, StreamOutputAdapter,
)
from groupdocs.search.highlighters import Highlighter, DocumentHighlighter, FragmentHighlighter
from groupdocs.search.dictionaries import (
    DictionaryRepository, SynonymDictionary, HomophoneDictionary,
    SpellingCorrector, AliasDictionary, StopWordDictionary,
)
from groupdocs.search.events import EventHub, FileIndexingEventArgs
from groupdocs.search.scaling import Indexer, Searcher, SearchNetworkNode, SearchNetworkStatus
from groupdocs.search.exceptions import SearchException, LicenseRestrictionException
```

## Index lifecycle (the core workflow)

`Index` is the entry point. The flow is always: **create/open an index → `add(...)` documents → `search(...)`**. An index is a folder on disk, so you build it once and reopen it on a later run by pointing `Index(...)` at the same folder. `update()` re-indexes only what changed; `optimize()` compacts the index for faster search.

```python
from groupdocs.search import Index

index = Index("./index")            # create, or reopen an existing index in this folder
index.add("./documents")            # extract text + index every supported file in the folder
result = index.search("contract")   # returns a SearchResult
print(result.document_count, result.occurrence_count)
```

**`Index(...)` constructor.** `Index(index_folder)` opens/creates an on-disk index. `Index(index_folder, IndexSettings(...))` configures it (in-memory index, stop words, index type, document filter, logger, custom extractors, …). Use `IndexRepository` to manage several indexes together. `Index` is disposable -- it works as a context manager (`with Index(...) as index:`) and has `dispose()`.

**Settings before documents.** Since 26.10, the dictionaries (`index.dictionaries`) and the settings that split text into words can change only while the index is empty. Changing one after `add()` raises "... cannot be changed because the index ... contains documents".

**`add(...)`** indexes a folder path or a single file path, a list of paths, a list of `Document` objects (`Document.create_from_file`, `create_from_stream`, `create_from_structure`, `create_lazy`), or extracted data, with optional `IndexingOptions` (`threads`, `is_async`, OCR and image indexing options). A file it cannot read does not raise: it is left out and reported through `index.events.error_occurred`.

**`search(query [, SearchOptions])`** parses a text query (see Query language) and returns a `SearchResult`: `.document_count`, `.occurrence_count`, iteration (`for found in result`), `.get_found_document(i)`, plus `.truncated`, `.warnings`, `.search_duration`.

## Query language

A single text query string can combine all of these:

| Form | Example | Meaning |
|---|---|---|
| Keyword | `Lorem` | match a word |
| Phrase | `"Lorem ipsum"` | match an exact phrase |
| Wildcard | `?orem`, `Lo*` | `?` = one character, `*` = any run |
| Regex | `^Lo.*` | regular expression (leading `^`) |
| Boolean | `Lorem AND ipsum`, `a OR b`, `a AND NOT b` | combine terms |
| Faceted | `Content: Lorem`, `FileName: report` | restrict to a field |

`SearchQuery` also builds queries programmatically (`SearchQuery.create_word_query`, `create_phrase_search_query`, `create_and_query`, `create_regex_query`, `create_wildcard_query`, `create_field_query`, …) for cases where string parsing is awkward. `create_and_query` / `create_or_query` take two or more queries, as separate arguments or one list. `create_regex_query(pattern, options)` takes `RegexOptions` from `groupdocs.search` (`RegexOptions.IGNORE_CASE | RegexOptions.CULTURE_INVARIANT`), which must include `IGNORE_CASE`; a Python `re` flag such as `re.IGNORECASE` is translated.

## Operations

### Keyword / phrase / wildcard / regex / boolean search

```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")
for query in ["contract", '"service agreement"', "?ontract", "^cont.*", "contract AND NOT draft"]:
    print(query, index.search(query).document_count)
```

### Fuzzy search (tolerate typos / OCR noise)

```python
from groupdocs.search import Index
from groupdocs.search.options import SearchOptions, SimilarityLevel

index = Index("./index")
index.add("./documents")
options = SearchOptions()
options.fuzzy_search.enabled = True
options.fuzzy_search.fuzzy_algorithm = SimilarityLevel(0.8)   # 0.0 loose .. 1.0 strict
options.fuzzy_search.only_best_results = True                 # optional
result = index.search("contarct", options)                    # matches "contract"
print(result.document_count)
```

`fuzzy_algorithm` accepts `SimilarityLevel(level)` or `TableDiscreteFunction(...)` for a custom edit-distance-by-length table.

### Synonym, homophone, word-forms and case-sensitive search

```python
from groupdocs.search import Index
from groupdocs.search.options import SearchOptions

index = Index("./index")
index.add("./documents")
options = SearchOptions()
options.use_synonym_search = True     # "make" also matches "create", "produce", …
options.use_homophone_search = True   # "right" also matches "write"
options.use_word_forms_search = True  # "contract" also matches "contracts"
print(index.search("contract", options).document_count)

case_sensitive = SearchOptions()
case_sensitive.use_case_sensitive_search = True
print(index.search("Service", case_sensitive).document_count)
```

Case-sensitive search does not combine with fuzzy, synonym, homophone or word-forms search, spelling or keyboard-layout correction: together they return an empty result, and `error_occurred` names the conflict.

Customize the dictionaries through `index.dictionaries` (a `DictionaryRepository`) before the first `add()`: `synonym_dictionary`, `homophone_dictionary`, `spelling_corrector`, `alias_dictionary`, `stop_word_dictionary`, `alphabet`, `character_replacements`, `word_forms_provider`. Each dictionary supports `import_dictionary(path)` / `export_dictionary(path)` / `clear()`.

### Inspect search results

```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")
result = index.search("contract")
print("documents:", result.document_count, "occurrences:", result.occurrence_count)
for found in result:                                   # FoundDocument
    print(found.document_info.file_path, found.occurrence_count, found.relevance)
    print("fields:", [f.field_name for f in found.found_fields])
```

### Highlight matches

```python
from groupdocs.search import Index
from groupdocs.search.common import FileOutputAdapter, StringOutputAdapter
from groupdocs.search.highlighters import DocumentHighlighter, FragmentHighlighter
from groupdocs.search.options import OutputFormat

index = Index("./index")
index.add("./documents")
found = index.search("contract").get_found_document(0)

# (a) whole document -> HTML file
index.highlight(found, DocumentHighlighter(FileOutputAdapter(OutputFormat.HTML, "./out.html")))

# (b) whole document -> in-memory string
adapter = StringOutputAdapter(OutputFormat.PLAIN_TEXT)
index.highlight(found, DocumentHighlighter(adapter))
text = adapter.get_result()

# (c) just the matching fragments
fh = FragmentHighlighter(OutputFormat.HTML)
index.highlight(found, fh)
for container in fh.get_result():
    print(container.field_name, container.count)
```

`OutputFormat` is `HTML` or `PLAIN_TEXT`. Output adapters: `FileOutputAdapter(format, path)`, `StringOutputAdapter(format)`, `StreamOutputAdapter(format, stream)`.

### List indexed documents and extract their text

```python
from groupdocs.search import Index
from groupdocs.search.common import StringOutputAdapter
from groupdocs.search.options import OutputFormat

index = Index("./index")
index.add("./documents")
for d in index.get_indexed_documents():    # list of DocumentInfo
    print(d.file_path)
print(index.get_indexed_paths())           # the added roots

adapter = StringOutputAdapter(OutputFormat.PLAIN_TEXT)
index.get_document_text(index.get_indexed_documents()[0], adapter)
print(adapter.get_result()[:200])
```

### Keep the index up to date

```python
import os
from groupdocs.search import Index
from groupdocs.search.options import UpdateOptions

index = Index("./index")
index.add("./documents")
index.update()        # re-index only documents changed since the last add/update
index.optimize()      # merge segments for faster subsequent searches
# delete() matches the paths the index stores, which are absolute
result = index.delete([os.path.abspath("./documents/draft-contract.txt")], UpdateOptions())
print(result.success_count)
```

### Reports & diagnostics

```python
from groupdocs.search import Index

index = Index("./index")
index.add("./documents")
index.search("contract")
for r in index.get_indexing_reports():
    print(len(r.indexed_documents), r.indexing_time)   # IndexingReport, one per batch
for r in index.get_search_reports():
    print(r.text_query, r.document_count)              # SearchReport, one per query
print(index.index_info.index_folder)                   # IndexInfo
```

## Events

`index.events` (an `EventHub`) raises .NET events; subscribe with `+=`, and a handler takes `(sender, args)`. `-=` removes the last subscription of that handler, `clear()` removes every handler added from Python, and `index.events.file_indexing = handler` raises `AttributeError` (use `+=`).

```python
from groupdocs.search import Index

index = Index("./index")

def on_file_indexing(sender, args):        # args: FileIndexingEventArgs
    if args.document_full_path.endswith(".tmp"):
        args.skip_indexing = True          # leave the file out

index.events.file_indexing += on_file_indexing
index.events.operation_progress_changed += lambda sender, args: print(args.processed_documents, "of", args.total_documents)
index.events.error_occurred += lambda sender, args: print("not indexed:", args.message[:80])
index.add("./documents")
index.events.file_indexing -= on_file_indexing
```

Events on `index.events`: `file_indexing`, `operation_progress_changed`, `operation_finished`, `optimization_progress_changed`, `error_occurred`, `password_required` (set `args.password`), `image_preparing`, `status_changed`, `search_phase_completed`. `Extractor` has `error_occurred`, `image_preparing`, `password_required`; the scaling `NodeEventHub` has the search-network events.

**Threads.** A handler runs on the thread that raises the event. With `IndexingOptions.threads` above 1 every `file_indexing` event runs on an engine thread, and with `is_async = True` (the call returns at once) every event does: wait for `operation_finished` with a `threading.Event`. An exception a handler raises on an engine thread is printed to stderr and ignored; on the calling thread it reaches the engine, which skips that document and reports it through `error_occurred`.

## Callbacks (interfaces as Python objects)

Write a plain class with the snake_case methods; do not subclass the generated interface class. One-method interfaces also take a function.

| Interface | Where it goes | Python shape |
|---|---|---|
| `ILogger` | `IndexSettings.logger` | `error(message)`, `trace(message)`; or `fn(level, message)` |
| `IFieldExtractor` | `IndexSettings.custom_extractors.add(...)`, `args.custom_extractor` | `extensions = [".log"]`, `get_fields(source)` returning `DocumentField`s or `(name, value)` pairs |
| `IContainerItemExtractor` | as above | plus `format_family = FormatFamily.ARCHIVE`, `get_container_items(source)` returning `ExtractedItemInfo`s or `(format_family, fields, item_info, inner_path)` |
| `IDocumentLoader` | `Document.create_lazy(kind, key, loader)` | `load_document()` returning a `Document`, `close_document()` |
| `IWordFormsProvider` | `index.dictionaries.word_forms_provider` | `get_word_forms(word)` -> list of str |
| `IWordSplitter` | `args.word_splitter` in `file_indexing` | `split(text)` -> iterable of str |
| `IOcrConnector` | `IndexingOptions.ocr_indexing_options.ocr_connector` | `recognize(context)` -> str or None; `context.image_stream.read()` |

`source` in `get_fields`/`get_container_items` is the file's path (`str`) or the engine's stream (`source.read()` -> bytes), valid during the call. `extensions` and `format_family` are read once, when the extractor is added. A lazy document calls its loader for `DocumentSourceKind.STREAM` and `STRUCTURE`; a `FILE` lazy document is read from its key as a path.

```python
from groupdocs.search import Index, IndexSettings

class LogExtractor(object):
    extensions = [".log"]

    def get_fields(self, source):
        text = open(source, encoding="utf-8").read() if isinstance(source, str) else source.read().decode("utf-8")
        return [("Content", text)]

class ConsoleLogger(object):
    def error(self, message):
        print("ERROR", message)

    def trace(self, message):
        pass

settings = IndexSettings()
settings.custom_extractors.add(LogExtractor())
settings.logger = ConsoleLogger()
index = Index("./index", settings)
index.add("./logs")
print(index.search("timeout").document_count)
```

## Licensing

```python
from groupdocs.search import License

License().set_license("path/to/license.lic")       # from a file
with open("path/to/license.lic", "rb") as f:        # or from a stream
    License().set_license(f)
```

Or auto-apply: `export GROUPDOCS_LIC_PATH="path/to/license.lic"` (a file or a URL). It is applied at the first call into the engine, and a licence that cannot be applied raises `LicenseError` from then on instead of falling back to evaluation mode (`GROUPDOCS_LIC_STRICT=0` makes it a warning). `Metered().set_metered_key(public_key, private_key)` activates metered billing; `Metered.get_consumption_quantity()` and `Metered.get_consumption_credit()` report usage.

**Evaluation mode** (no licence): an index takes at most 100 documents (the rest are left out and reported through `error_occurred`), and only the first 10 found documents can be read: `result.get_found_document(10)` raises `LicenseRestrictionException`. A 30-day full license is free: https://purchase.groupdocs.com/temporary-license

**Windows:** applying a licence makes the first call take 15-30 seconds, once per process (the engine's licence check; the same in plain .NET). Reuse the process.

## API Reference

### Index

| Method | Returns | Description |
|---|---|---|
| `Index(index_folder [, IndexSettings])` | | Create or open an on-disk index. Disposable / context manager. |
| `add(path [, IndexingOptions])` | `None` | Index a folder, a file, a list of paths, `Document`s or extracted data. |
| `update([UpdateOptions])` | `None` | Incrementally re-index documents that changed. |
| `optimize([OptimizeOptions])` | `None` | Merge index segments to speed up search. |
| `merge(index)` / `merge(repository)` | `None` | Merge another index into this one. |
| `search(query [, SearchOptions])` | `SearchResult` | Run a text query, a `SearchQuery` or a `SearchImage`. |
| `search_next(chunk_search_token)` | `SearchResult` | Continue a chunked search. |
| `highlight(found_document, highlighter [, HighlightOptions])` | `None` | Render highlights via an `OutputAdapter`. |
| `get_indexed_documents()` | list of `DocumentInfo` | All documents currently in the index. |
| `get_indexed_paths()` | list of `str` | Root paths that were added. |
| `get_document_text(document_info, adapter)` | `None` | Extract a document's indexed text into an adapter. |
| `get_indexing_reports()` / `get_search_reports()` | lists | Indexing and search diagnostics. |
| `delete(paths, UpdateOptions)` | `DeleteResult` | Remove documents from the index. |
| `check_segments(fix)` | `None` | Verify (and optionally repair) index integrity. |

`Index` properties: `index_info` (`IndexInfo`), `index_settings` (`IndexSettings`), `dictionaries` (`DictionaryRepository`), `repository` (`IndexRepository`), `events` (`EventHub`).

### Search options & results

| Type | Notes |
|---|---|
| `SearchOptions` | `use_case_sensitive_search`, `use_synonym_search`, `use_homophone_search`, `use_word_forms_search`, `fuzzy_search`, `spelling_corrector`, `keyboard_layout_corrector`, `max_total_occurrence_count`, `max_occurrence_count_per_term`, `search_document_filter`. |
| `FuzzySearchOptions` | `enabled`, `fuzzy_algorithm`, `only_best_results`, `only_best_results_range`, `consider_transpositions`. |
| `SearchResult` | `document_count`, `occurrence_count`, iteration, `get_found_document(i)`, `truncated`, `warnings`, `search_duration`, `next_chunk_search_token`. |
| `FoundDocument` | `document_info`, `occurrence_count`, `relevance`, `found_fields`, `terms`. |
| `DocumentInfo` | `file_path`, `file_type`, `format_family`, `inner_path`. |

### Indexing & maintenance options

| Type | Notes |
|---|---|
| `IndexSettings` | `in_memory_index`, `use_stop_words`, `index_type`, `document_filter`, `text_storage_settings`, `search_threads`, `logger`, `custom_extractors`, `replace_unpaired_surrogates`, `skip_blended_only_terms`. |
| `IndexingOptions` / `UpdateOptions` | `threads`, `is_async`, `ocr_indexing_options`, `image_indexing_options`, `cancellation`. |
| `MergeOptions` / `OptimizeOptions` / `DeleteOptions` | Index maintenance. |
| `HighlightOptions` | Highlight rendering options. |

### Highlighters & adapters

| Type | Notes |
|---|---|
| `DocumentHighlighter(output_adapter)` | Highlight an entire document. |
| `FragmentHighlighter(OutputFormat)` | Highlight matching fragments; `get_result()` -> fragment containers. |
| `FileOutputAdapter(OutputFormat, path)` / `StringOutputAdapter(OutputFormat)` / `StreamOutputAdapter(OutputFormat, stream)` | Where highlight and text output goes. |

## Key Patterns

- **Properties**: use `snake_case` -- auto-mapped to .NET `PascalCase`. A misspelled property raises `AttributeError` with a "Did you mean" hint instead of being stored
- **Index is a folder**: point `Index(...)` at the same folder to reopen a persisted index; `update()` re-indexes only changes
- **Settings before documents**: dictionaries and word-splitting settings change only while the index is empty
- **One query string, many forms**: keyword, `"phrase"`, `?`/`*` wildcards, `^regex`, `AND/OR/NOT`, and `Field:` facets all parse from one string
- **Options objects**: tweak `SearchOptions` sub-objects in place (e.g. `options.fuzzy_search.enabled = True`) before passing to `search`
- **Enums**: UPPER_CASE members (`OutputFormat.HTML`, `DocumentSourceKind.STREAM`); `FormatFamily` is a class of constants (`FormatFamily.ARCHIVE`)
- **Collections**: `for found in result`, `for d in index.get_indexed_documents()` and `len(...)` work
- **Events**: `index.events.<event> += handler(sender, args)`; mind the threads (see Events)
- **Callbacks**: plain Python objects or functions for the engine's interfaces (see Callbacks)
- **Exceptions**: catch `SearchException` / `LicenseRestrictionException` (both subclass the product-root `GroupDocsSearchException`, importable from `groupdocs.search`); `LicenseError` is a licence that could not be applied. `e.message` is the .NET message alone; `str(e)` adds the .NET type and stack trace
- **Params arrays**: a static method takes them as separate arguments or one list (`DocumentFilter.create_file_extension(".txt", ".pdf")`); an instance method or constructor takes one list (`batch.add(path, ["important"])`, `TableDiscreteFunction(1, [Step(5, 2)])`)

## Platform Requirements

| Platform | Requirements |
|---|---|
| Windows | None |
| Linux | glibc 2.27+, `apt install libicu-dev libgdiplus` (ICU is required; libgdiplus for ODP and PPT files). No fonts needed |
| macOS | 13 or newer, `brew install mono-libgdiplus` (for presentations) |

## Troubleshooting

**Only 100 documents indexed, or `LicenseRestrictionException: Only first 10 results can be returned`** -- evaluation mode. Apply a license with `License().set_license(...)` or set `GROUPDOCS_LIC_PATH`; a free 30-day license is at https://purchase.groupdocs.com/temporary-license

**`... cannot be changed because the index ... contains documents`** -- set dictionaries and word-splitting settings before the first `add()` (26.10 behaviour)

**A presentation is not indexed; `error_occurred` reports `The type initializer for 'Gdip' threw an exception` or a `NullReferenceException`** -- install libgdiplus: `sudo apt install libgdiplus` (Linux) / `brew install mono-libgdiplus` (macOS)

**A handler's exception does not stop indexing** -- on an engine thread it is printed and ignored; to skip a document set `args.skip_indexing = True`; to stop an operation use `IndexingOptions.cancellation`

**The first call takes 15-30 seconds** -- Windows, with a licence applied: the engine's licence check, once per process

**`DOTNET_SYSTEM_GLOBALIZATION_INVARIANT` errors, or the process aborts with `Couldn't find a valid ICU package`** -- do NOT set this variable. Install ICU: `sudo apt install libicu-dev`

**`is not a supported wheel on this platform`** -- pip is older than 20.3 (`python -m pip install --upgrade pip`), or the OS is older than glibc 2.27 / macOS 13

**Still stuck?** Post your question at https://forum.groupdocs.com/c/search/ -- the development team responds directly.
````
