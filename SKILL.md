---
name: hot-news-brief
description: Produce concise, evidence-linked briefs on recent business news and trends using a configured read-only news-search MCP. Use for the latest developments in a company, industry, product, technology, or commercial event; quick decision briefs; and content-topic discovery.
---

# Hot News Brief

Deliver a short, source-linked briefing from public news published in the last seven days. This skill is platform-agnostic and contains no credentials.

## Workflow

1. Identify the topic and desired output. Use `news_list` by default; use `decision_brief`, `content_radar`, or `both` only when requested.
2. Choose `fast` mode by default. Use `deep` mode only when explicitly requested; it covers six distinct search angles.
3. Confirm that the configured read-only connector exposes equivalents of `list_platforms` and `search_platform_content`. Stop before collection if either is unavailable.
4. Reject political elections, military conflict, sovereignty disputes, weapons deployment, and intelligence topics. Do not search a rejected topic.
5. Search with a strict seven-day freshness window. In fast mode make one query; in deep mode plan six non-overlapping queries.
6. Keep only records with a title, HTTP(S) URL, and valid publication date. Exclude future-dated or out-of-window records, normalize URLs, deduplicate stories, and cap each domain at three entries.
7. Rank candidates by relevance, source quality, recency, impact, and novelty. Keep at most 20 distinct stories.
8. Write one to three evidence-based summaries. Add decision implications or content angles only when they are supported by the selected stories.

## Guardrails

- Copy titles, publication dates, sources, and URLs exactly from returned records.
- Never invent links, quotes, popularity metrics, causal claims, or facts not supported by a selected story.
- Do not expose credentials, authorization headers, connector responses, or account information.
- Treat the result as a brief of collected coverage, not a claim of complete news coverage.
- Use `scripts/compact_search_result.mjs` only when the host has persisted an oversized connector response; otherwise process results in memory.

## Output format

Respond directly with:

1. A title and a statement that coverage is restricted to the last seven days.
2. One to three short summaries.
3. A numbered news list using `N. [Title](URL) — Source · YYYY-MM-DD`.
4. Optional decision implications or content angles, each tied to one or more listed stories.
5. A short coverage note explaining the search mode and any material gaps.

If no qualifying records remain, state: “No qualifying news was found in the last seven days.”

## Connector configuration

Copy `mcp.example.json` to the configuration location required by your MCP host. Set the referenced environment variable locally or through a secret manager. Do not commit a populated connection file.
