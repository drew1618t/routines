# Pre-Market Portfolio Triage

## Task

Run a pre-market news triage for all positions in
`investing/positions.json`. Apply the methodology in
`investing/SKILL.md`.

## Process

1. Read `shared/about-drew.md` for context and communication style.
2. Read `investing/SKILL.md` in full for methodology.
3. Read `investing/positions.json` — this is the authoritative list of
   current holdings. Each entry has a thesis and `key_metrics_to_watch`.
   Tickers with empty thesis get triaged anyway; flag them at the bottom
   of the email under **Missing thesis metadata**.

For each ticker in `positions.json`:

4. Search the web for news from the last 18 hours related to:
   - The ticker symbol
   - The company name
   - Direct competitors (if material news affects the sector)
   - The specific metrics listed in key_metrics_to_watch (if present)
5. Filter for material information only. Ignore routine analyst
   price target adjustments, options flow commentary, and generic
   market commentary.
6. Rate each position URGENT / NOTABLE / QUIET per SKILL.md scheme.

Then:

7. Check macro conditions:
   - US NASDAQ futures direction and magnitude
   - 10-year Treasury yield movement
   - Any Fed commentary or economic data overnight
   - Sector rotation signals affecting growth stocks
8. Check for broad sector news affecting multiple positions
   (e.g., semiconductor export controls affecting MU, ALAB, CRDO;
   crypto price moves affecting IREN; AI ad-tech dynamics affecting APP).

## Output format

Send via Gmail to drew1618t@gmail.com with subject:
`Portfolio Triage YYYY-MM-DD`

Body structure:

```
# Portfolio Triage — [Date]

## URGENT
(Only include this section if there are URGENT items)

### [TICKER] — [One-line summary]
- What happened: [2-3 sentences]
- Why it matters: [impact on thesis or watched metrics]
- Suggested attention: [what to look at / decide today]
- Sources: [links]

## NOTABLE

### [TICKER] — [One-line summary]
- [1-2 sentence summary]
- Sources: [links]

## QUIET
[TICKER], [TICKER], [TICKER] — no material news

## Macro / Sector

- NASDAQ futures: [direction, magnitude]
- 10Y yield: [level, change]
- Notable: [any macro items worth flagging]

## Cross-position themes
[If sector news affects multiple positions, note here]

## Missing thesis metadata
[Tickers in positions.json with empty thesis / key_metrics_to_watch — list only]
```

## Holiday handling

If today is a US market holiday, produce an abbreviated version:
- Note the holiday at the top
- Skip futures and yield sections
- Focus on international news and any scheduled company events
- Skip the QUIET section

## Weekend handling

Should not run on weekends (schedule handles this) but if
manually triggered, produce a week-ahead preview instead:
earnings calendar, known catalysts, macro data releases.
