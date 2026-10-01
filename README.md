# Greenlit Books MCP server

The [Greenlit Books](https://greenlitbooks.com) catalog as a remote MCP server: practical books on working with AI, held to one standard, every claim is something you can check. All titles are live on Amazon and free to read with Kindle Unlimited.

- **Endpoint**: `https://greenlitbooks.com/api/mcp` (streamable HTTP, stateless, no auth)
- **Registry name**: `com.greenlitbooks/catalog` (version 1.5.0)
- **Docs for humans and agents**: <https://greenlitbooks.com/developers>
- **Plain JSON API** (same data, plain GET): <https://greenlitbooks.com/api/v1> with an [OpenAPI 3.1 spec](https://greenlitbooks.com/api/v1/openapi.json)

## Tools

| Tool | What it does |
|---|---|
| `find_answer_in_books` | Ask a question in your own words and get the passages that answer it, from what the books publish free: chapter one of every book (by section), each book's named concept and its FAQ, the glossary, the claim ledger and the guides. Matched by words and by meaning. Each passage comes with the exact URL to quote, the finished citation, and the page to read for more; a question the free pages do not answer returns nothing. |
| `search` | Search everything on greenlitbooks.com: safety verdicts on AI agents, MCP servers and coding tools ("is X safe"), field notes, guides and the books. Returns ids to pass to `fetch`. |
| `fetch` | Read one page by an id from `search`: the page as markdown, with its canonical URL to cite and its dates. |
| `search_greenlit_books` | Ranked full-text search over the catalog. Filters: `series`, `audience`, `limit`. |
| `get_book` | One book in full: chapters, word count, free chapter one, Amazon Kindle and paperback links, prices, Kindle Unlimited status, related reading. |
| `find_books_for_topic` | Match a topic phrase (agent reliability, prompt injection, human in the loop, durable execution, ...) against the curated topic map. |
| `get_reading_path` | One series with its books in reading order. |
| `get_concept` | The named question a book answers, with its short answer, full explanation, and a links block (free chapter, Amazon, cite-as). |
| `get_free_chapter` | The complete text of chapter one of any book, free to read and to quote, with the chapter list and where to get the rest. |
| `cite_source` | The finished citation for a book, one of its free chapters, or one of its coined terms: the line to print, the canonical URL, the markdown twin of the same page, and the date it was last verified. It asserts nothing about the content, only how to name the source. |
| `define_term` | The glossary: a term the books coin or pin down (the green lie, blast radius, gate faith), defined in the book's own words, with the chapter, the other books that use it, a checkable claim, related terms, and links to cite and hand off. Unknown term returns the full list with aliases. |
| `check_claim` | Search the claim ledger for claims bearing on a statement, before asserting it. Each hit carries the claim, a citation URL resolving to that exact sentence, what kind of statement it is and the one-line rule for quoting that kind, a citation already written in prose and BibTeX, and for every external source both what it establishes and what it does not. Filter to `basis=external` when only a published result will do. Returns nothing when the shelf has no claim on the subject, which is an answer rather than a failure. |

## Connect

**Claude Code**

```bash
claude mcp add --transport http greenlit-books https://greenlitbooks.com/api/mcp
```

**Claude Desktop / claude.ai**: Settings > Connectors > Add custom connector, URL `https://greenlitbooks.com/api/mcp`.

**Cursor / any client with JSON config**

```json
{
  "mcpServers": {
    "greenlit-books": {
      "url": "https://greenlitbooks.com/api/mcp"
    }
  }
}
```

## Notes

- Read-only. No accounts, no keys, no rate-limit registration.
- The server is stateless; every response is derived from the live catalog.
- Amazon links are clean `/dp/` product URLs.
- The catalog intentionally omits publication dates, page counts, and ISBNs: it carries no verified source for them, and nothing is invented.
- Glossary definitions are quoted, not written: each is verified verbatim against the book's manuscript when the site is built. Terms borrowed from another field (blast radius, span of control) say so.
- Every `define_term` and `get_concept` result ends with a `links` block. Cite the glossary or concept URL; send readers to `freeChapter` first, then `amazonKindle`.

This repository holds the server manifest and documentation. The server itself runs on greenlitbooks.com.
