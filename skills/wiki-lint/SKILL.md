---
name: wiki-lint
description: Periodic maintenance pass on the research wiki — contradictions, stale claims, orphans, missing links, data gaps
---

Run a wiki lint pass on your `research/wiki/` knowledge base (see [`SCHEMA.md`](../../SCHEMA.md) for the wiki's conventions — read it first, along with `research/wiki/index.md`). This is meant to run as an automated, unattended task: execute autonomously and report findings, don't ask clarifying questions.

## Steps

1. Read `research/wiki/index.md` for the full page list, then read every content page in `research/wiki/` (every `.md` file except `index.md` and `log.md`).

2. Check for, per SCHEMA's Lint definition:
   - Contradictions between pages
   - Stale claims superseded by newer sources or by other wiki pages
   - Orphan pages (zero inbound `[[wikilinks]]` from other content pages)
   - Concepts/entities referenced in `[[double-bracket]]` form that don't resolve to an actual file (broken links), or mentioned repeatedly in prose but lacking their own page
   - Missing cross-references between obviously related pages
   - Data gaps a web search could fill

3. **Auto-fix only mechanical, judgment-free issues** — this is the one thing you're allowed to write directly without asking:
   - Broken wikilinks where the correct target is unambiguous (e.g. `[[Some Page]]` should be `[[some-page]]` because `some-page.md` exists and is clearly the same entity) — fix directly.
   - A page that's a one-directional orphan where the fix is obviously just adding a "See also" backlink from a page that already references it — fix directly (add the missing backlink, don't rewrite existing content).

   Do **not** touch anything that requires judgment: don't resolve contradictions, don't decide what's "stale" and remove it, don't invent new pages, don't rewrite prose. Those go in the findings report for you to decide.

4. Write a findings report as a new entry in `research/wiki/log.md` (prepend after the header, following the existing format: `## [YYYY-MM-DD] lint | [brief summary]`), listing: what was found in each category (named pages, not vague generalities), what was auto-fixed and why it was safe, and a short "if you fix nothing else, do these 3" priority list for anything requiring judgment.

5. Post a brief summary to the conversation: how many pages reviewed, how many mechanical fixes applied, and the top 1–3 judgment-call findings worth a look. End with `<run-summary>one or two sentences on what was found and what, if anything, needs a decision</run-summary>`.

Never invent facts, never delete existing wiki content, never ingest new sources — this is maintenance only.

## Running this as a scheduled task

Save this file under your scheduled-tasks directory alongside a copy of (or path to) `SCHEMA.md`, then create the scheduled task pointing at it. Monthly is the natural cadence for a small-to-medium wiki — contradictions and orphan pages accumulate slowly.
