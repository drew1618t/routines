# Handoff: add daily stock-triage post to this site

Paste this into a Claude Code session running in your **website repo on
the Pi**. It explains a routine that currently runs elsewhere and what
you want this repo to start receiving.

---

## What the routine is

A daily pre-market stock-news triage. Every weekday at 10:00 GMT-3 it:

1. Reads a list of tickers from
   `github.com/drew1618t/routines` → `investing/positions.json`
2. Searches the web for material news in the last ~18 hours on each
   ticker (filings, earnings, regulatory action, customer wins,
   competitor moves, sector news affecting the position).
3. Rates each ticker URGENT / NOTABLE / QUIET against a fixed methodology
   (see `investing/SKILL.md` in that repo).
4. Adds a macro section (NASDAQ futures, 10Y yield, Fed news) and
   cross-position sector themes.
5. **Currently emails the result to drew1618t@gmail.com via Gmail.**

The full prompt that drives the run lives at
`investing/prompts/pre-market-triage.md` in the routines repo. Read it
to understand the exact output shape — that's the body you'll be
publishing.

## What I want this repo to do

Instead of emailing, **publish each day's triage as a post on this
site** so I can browse the archive and stop cluttering my inbox.

Concretely, add:

1. **A content directory for triage posts.** Pick the conventional
   location for this site's static-site-generator — e.g.,
   `content/triage/` for Hugo, `_posts/triage/` for Jekyll,
   `src/content/triage/` for Astro. Each run drops a file named
   `YYYY-MM-DD.md` there with the routine's output as the body and
   appropriate front-matter (title: "Portfolio Triage — YYYY-MM-DD",
   date, tags: ["triage", "investing"], any layout the theme expects).

2. **A list/archive page** showing all triage posts in reverse-chrono
   order with date and one-line summary. If the theme already lists
   posts by tag or section, lean on that — don't build a custom page
   unless needed.

3. **An execution script** at `scripts/run-triage.sh` (or whatever this
   repo's convention is) that:
   - `cd`s into a checkout of the routines repo (clone it under
     `~/routines` if not present; pull latest on each run)
   - Invokes `claude` CLI non-interactively with the prompt
     `Run investing/prompts/pre-market-triage.md` BUT modify the run so
     that instead of emailing, Claude writes the output to
     `<this-repo>/content/triage/$(date +%F).md` with proper
     front-matter
   - Rebuilds the site (whatever the site's normal build command is)
   - Logs success/failure to `~/triage.log`

   Easiest way to "modify the run" is a wrapper prompt that includes
   the original prompt by reference and overrides the "Send via Gmail"
   step with "Write to `<path>` with front-matter `<schema>`". Put that
   wrapper prompt in this repo at `scripts/triage-prompt.md` and have
   the cron call point at it.

4. **A crontab entry** for weekdays at 10:00 local time:
   `0 10 * * 1-5 /home/<user>/<repo>/scripts/run-triage.sh`
   Add it via `crontab -e` and confirm with `crontab -l`. Don't
   overwrite existing entries.

## Constraints / notes

- The site sits behind **Cloudflare Access**, so it's fine to publish
  the posts at a normal URL — they're auth-gated. No need for a hidden
  route.
- The Pi already has `claude` CLI installed and authenticated (assume
  this; if `which claude` returns nothing, stop and tell me).
- Don't change the underlying prompt in the routines repo. Wrap it.
- Keep the existing email-based routine on claude.ai/code running for
  one week as a fallback so we can compare outputs side-by-side. After
  that I'll disable it.

## What to confirm before building

1. Which static-site generator this repo uses and where its content
   lives.
2. The site's build command and how it gets served (nginx? `hugo
   server`? Cloudflare Pages build?).
3. Whether `claude` CLI on the Pi can authenticate non-interactively
   (cron has no TTY). If it needs a token, surface that as a setup
   step.

Show me the plan before writing files.
