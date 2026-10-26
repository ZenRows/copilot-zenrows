# Changelog

## 0.1.1

- The `extract` skills no longer say `fallback_autoparse` retries with autoparse: it has had no effect since the Zenrows MCP server removed that fallback. They now handle `REQS007` by its `reason`: `domain_not_prepared` (prepare the domain with `prepare_domain`, then wait for `prepared_domain_status`), `page_not_recognised` and `domain_not_enabled`. `mode='autoparse'` is marked deprecated, and `include_html` is listed.

## 0.1.0

- First release for GitHub Copilot: the Zenrows MCP server (OAuth sign-in) and nine skills: `using-zenrows`, `getting-started`, `scrape-webpage`, `extract-structured-data`, `batch`, `crawl`, `map`, `browser-automation` and `capture-api-calls`. The skills match [openai-zenrows](https://github.com/ZenRows/openai-zenrows), with setup steps for Copilot. `crawl` runs Zenrows Crawl as a background job (`crawl_create`, `crawl_wait`, `crawl_results`).
