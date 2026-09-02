# Greenlit Books MCP server

The [Greenlit Books](https://greenlitbooks.com) catalog as a remote MCP server: practical books on working with AI, held to one standard, every claim is something you can check. All titles are live on Amazon and free to read with Kindle Unlimited.

- **Endpoint**: `https://greenlitbooks.com/api/mcp` (streamable HTTP, stateless, no auth)
- **Registry name**: `com.greenlitbooks/catalog` (version 1.2.0)
- **Docs for humans and agents**: <https://greenlitbooks.com/developers>
- **Plain JSON API** (same data, plain GET): <https://greenlitbooks.com/api/v1> with an [OpenAPI 3.1 spec](https://greenlitbooks.com/api/v1/openapi.json)

## Tools

| Tool | What it does |
|---|---|
| `search_greenlit_books` | Ranked full-text search over the catalog. Filters: `series`, `audience`, `limit`. |
| `get_book` | One book in full: chapters, word count, free chapter one, Amazon Kindle and paperback links, prices, Kindle Unlimited status, related reading. |
| `find_books_for_topic` | Match a topic phrase (agent reliability, prompt injection, human in the loop, durable execution, ...) against the curated topic map. |
| `get_reading_path` | One series with its books in reading order. |
| `get_concept` | The named question a book answers, with its short answer, full explanation, and a links block (free chapter, Amazon, cite-as). |
| `get_free_chapter` | The complete text of chapter one of any book, free to read and to quote, with the chapter list and where to get the rest. |
| `define_term` | The glossary: a term the books coin or pin down (the green lie, blast radius, gate faith), defined in the book's own words, with the chapter, the other books that use it, a checkable claim, related terms, and links to cite and hand off. Unknown term returns the full list with aliases. |

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
