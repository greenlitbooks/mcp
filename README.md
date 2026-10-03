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

## Install as a plugin

This repository is also a plugin for Cursor, Grok Bot and Grok Build. It contains the connection to the server above and four skills that tell the agent when to use it. There are no rules, commands, hooks or scripts, so there is no code to run on your machine.

- **Cursor and Grok Bot**: open the plugin marketplace, search for **Greenlit Books**, and install.
- **Grok Build**: run `/marketplace`, search for **Greenlit Books**, and install.

| Skill | Use it when you ask |
|---|---|
| `answer-from-the-books` | How, why or what about building, running, securing or trusting AI agents, or about an idea such as the green lie, blast radius or gate faith. Answers from the free chapters, concept pages, glossary and claim ledger, with a citation for each passage. |
| `check-a-claim` | Whether a statement about AI agents is true, or where a claim comes from. Searches the claim ledger and says what each source does and does not establish. |
| `check-ai-agent-safety` | Whether a named AI agent, MCP server or coding assistant is safe to use. Reports the dated verdict and the version it covers. |
| `recommend-a-book` | Which book to read for a topic, in what order, or where to read it free. |

| File | Read by |
|---|---|
| `.cursor-plugin/plugin.json`, `mcp.json` | Cursor and Grok Bot |
| `.grok-plugin/plugin.json`, `.mcp.json` | Grok Build |
| `skills/*/SKILL.md`, `assets/logo.svg` | both |

### Network and credentials

- The plugin connects to one endpoint, `https://greenlitbooks.com/api/mcp`, over HTTPS (streamable HTTP).
- No credentials. There is no sign-in, no OAuth and no API key, and the plugin reads nothing from your disk.
- Every tool is read-only. Requests are handled as described in the [privacy policy](https://greenlitbooks.com/privacy).

## Notes

- Read-only. No accounts, no keys, no rate-limit registration.
- The server is stateless; every response is derived from the live catalog.
- Amazon links are clean `/dp/` product URLs.
- The catalog intentionally omits publication dates, page counts, and ISBNs: it carries no verified source for them, and nothing is invented.
- Glossary definitions are quoted, not written: each is verified verbatim against the book's manuscript when the site is built. Terms borrowed from another field (blast radius, span of control) say so.
- Every `define_term` and `get_concept` result ends with a `links` block. Cite the glossary or concept URL; send readers to `freeChapter` first, then `amazonKindle`.

This repository holds the server manifest, the plugin manifests and the documentation. The server itself runs on greenlitbooks.com.

## License

[MIT](LICENSE). The books' free chapters and the pages the server returns are published on greenlitbooks.com under that site's own terms.
