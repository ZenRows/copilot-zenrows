---
name: crawl
description: Collect the content of many pages under one site or section by following internal links from a starting URL. Use when the user wants more than one page, for example "all the docs under /guide" or "every blog post on this site". Not for listing URLs without fetching their content (use map), not for a single page (use scrape-webpage), and not for a list of URLs the user already has (use batch).
---

# Crawl from a seed URL

Discover and fetch internal pages starting from one seed, staying on the same host and path prefix. The agent runs the loop in conversation.

## Defaults

- `max_pages`: 25
- `max_depth`: 2
- Scope: same host and same path prefix as the seed.

## Instructions

1. Confirm the seed URL and, if the user implied limits, the `max_pages` and `max_depth` to use.
2. Fetch the seed with `scrape(url, response_type='markdown')`. The markdown keeps the page's links, so read the next URLs from it. If you only need the links and not the content, use `scrape(url, outputs='links')` instead, which returns a JSON list of links without the page text.
3. Filter the returned links to the same host and the same path prefix as the seed. Drop off-site links, anchors, and already-seen URLs.
4. Enqueue the surviving links. Repeat steps 2 and 3 for each, increasing depth, until you reach `max_pages` or `max_depth`.
5. If you can write files (for example in Copilot CLI or VS Code agent mode), save each fetched page as one JSON line to `./.zenrows/crawl-<host>-<timestamp>.jsonl`, with at least `{ "url", "depth", "content" }`, and do not read the whole file back into context.
6. Summarize from what you fetched.

## Notes

- A crawl multiplies requests, so `max_pages` and `max_depth` are the cost control. Keep them as low as the task allows.
- For URL discovery without fetching content, use the `map` skill instead. It is far cheaper.
- Once the URL list is known (for example from `map`), use the `batch` skill rather than this loop. Batch submits them as one managed job with server-side retries.
