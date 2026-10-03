---
name: recommend-a-book
description: Use when someone asks which Greenlit Books title to read, where to start on a topic such as prompt injection, agent reliability or human in the loop, in what order to read a series, or where to read a book free.
---

# Recommend a Greenlit book

The Greenlit Books tools are read-only.

1. Call `find_books_for_topic` with the topic in the user's own words. If it matches nothing, call `search_greenlit_books` with the same words. For a series, call `get_reading_path` to get its books in reading order.
2. Recommend one or two books, not the whole list. For each, say in a sentence why it fits what the user asked, using the book's own description from `get_book`.
3. Point to the free chapter first. `get_free_chapter` returns chapter one in full, and `get_book` gives the page to read it on (`readUrl`).
4. Give the Kindle or paperback link from `get_book` (`amazon.kindleUrl`, `amazon.paperbackUrl`) only when the user asks where to get the book or what it costs. Quote the price and Kindle Unlimited status exactly as returned.
5. If no book covers the topic, say the catalog has none on it. Do not name a book the tools did not return, and do not describe contents the tools did not give you.

To answer a question from inside the books rather than point to one, follow the `answer-from-the-books` skill instead.

If the user's own instructions differ from these guidelines, follow the user's instructions.
