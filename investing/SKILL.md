---
name: investing-analysis
description: Surface material overnight news for US equity positions in a
  Saul-methodology growth portfolio. Use when running pre-market or
  post-market triage over positions.json. Triggers on prompts mentioning
  tickers, positions.json, portfolio triage, or pre-market/post-market
  news review.
---

# Investing News Triage Skill

## Purpose

This skill surfaces **new news** from the last ~24 hours that is
relevant to an investor in each position. It is **not** a full position
evaluation — no rule-scoring, no dimension-weighting, no buy/sell calls.
Deeper evaluation belongs in a separate weekly-review routine.

## Methodology context

Portfolio follows Saul's growth methodology: concentrated (10-15
positions) in high-growth companies, target ~27% CAGR. Every position
has a thesis and a short list of metrics that matter. News is relevant
when it could move the thesis or move the watched metrics.

## Core files (in investing/ directory)

- `positions.json` — current holdings with per-position thesis and
  `key_metrics_to_watch`. Use these to decide relevance.
- `exemplars/` — reference write-ups for tone and depth calibration
  (populated over time).

## Triage process

For each position in `positions.json`:

1. Search the web for news in the last ~18-24 hours on the ticker,
   company name, and the specific items in `key_metrics_to_watch`.
2. Filter out noise: routine analyst price-target adjustments, options
   flow commentary, generic market chatter, promotional content,
   SEO-farm "news" sites.
3. Decide materiality and assign one rating below.

## Urgency rating scheme

- **URGENT** — Thesis-affecting or metric-moving. Requires same-day
  attention. Examples: earnings print with guidance cut, management
  departure, major customer loss, regulatory action, acquisition
  announcement, material cybersecurity incident, a number in
  `key_metrics_to_watch` coming in off-trend.

- **NOTABLE** — Worth knowing, no immediate action needed. Examples:
  analyst upgrade/downgrade with new thesis, competitor announcement
  that doesn't directly threaten the thesis, sector news with indirect
  exposure, insider transactions below materiality thresholds.

- **QUIET** — Nothing material. List ticker only in the output.

## Output conventions

- Lead with urgency rating.
- State concretely what happened and why it matters for this position's
  thesis or watched metrics.
- Cite exact numbers from sources, not paraphrases.
- No hedging. If the read is clear, say so. If genuinely uncertain,
  flag it explicitly.
- Source links at the end of each item, not inline.

## Source quality hierarchy

Prefer in this order:
1. Company IR pages, SEC filings, earnings transcripts
2. Reputable financial press (Reuters, Bloomberg, WSJ, FT)
3. Trade publications specific to the sector
4. Analyst notes (with author named if available)

Skip: unverified social media, promotional newsletters, pure
speculation pieces, SEO-farm financial "news" sites.
