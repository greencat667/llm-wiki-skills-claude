---
name: wiki-ripple-recommender
description: Periodic scan of new raw captures and inbox digests — recommends what durable insight should ripple into the research wiki
---

Run the wiki ripple-recommender for your `research/` knowledge base (see [`SCHEMA.md`](../../SCHEMA.md) for conventions — read it first, especially the "Memory ↔ wiki boundary" section). This is meant to run as an automated, unattended task: execute autonomously, don't ask clarifying questions. Your job is to surface **recommendations only** — never write new wiki pages or edit existing ones. Deciding what actually ripples into `research/wiki/` stays a human choice.

This file is a template. Fill in the ✏️ section below with your own upstream sources before using it.

## ✏️ Your upstream sources

List whatever periodically drops raw material into `research/raw/` or `research/inbox/` that nobody is otherwise reviewing on a schedule — a session-capture task, a trend-monitoring task, a specific project log that tends to surface durable material, an inbox folder you dump links into. If you don't have any dedicated upstream automation yet, just point this at whatever folders you manually drop notes/clippings into.

*Example:*
- A daily session-capture task writing transcripts to `research/raw/sessions/`
- A weekly trend-monitoring task dropping a digest into `research/inbox/`
- A specific project's log file that has repeatedly surfaced material worth generalizing

## Steps

1. Read `research/wiki/log.md` to find the date of the last `recommend` entry (search for `| recommend |`). If none exists, look back 7 days from today.

2. List files in `research/raw/sessions/`, `research/inbox/`, and any other sources from your ✏️ list above, with a modified/created date after that last-reviewed date (use `find ... -newer`, or check filenames if they're date-prefixed).

3. For each new file:
   a. Read it.
   b. Read `SCHEMA.md`'s "Current territories" list and `research/wiki/index.md` to understand what the wiki already covers.
   c. Apply the boundary test from `SCHEMA.md`: "would this still matter if you forgot which project/week it came from?" Most operational captures (admin, status updates, one-off scheduling/logistics, navigating a specific meeting) should be excluded — say so briefly and why. Only flag content that's a durable concept, entity, pattern, or synthesis that extends or challenges something already in the wiki, or opens a genuinely new territory.
   d. For anything that clears that bar, name: the source file, a suggested wiki page title, suggested page type (concept / entity / source-summary / synthesis / doctrine), which existing wiki pages it would connect to, and a one-sentence reason it's durable rather than operational.

4. Also check any project-specific log you listed in your ✏️ sources above for new entries since your last run, and apply the same test to those.

5. Write the recommendations to a new file: `research/inbox/YYYY-MM-DD-ripple-recommendations.md` with frontmatter:
   ```yaml
   ---
   title: Ripple Recommendations — YYYY-MM-DD
   type: inbox-digest
   source-type: generated
   generated-by: wiki-ripple-recommender (scheduled task)
   date: YYYY-MM-DD
   tags: [ripple-recommendations, wiki-maintenance]
   ---
   ```
   Then list the recommendations from steps 3–4, and a short "excluded, and why" section for anything you deliberately did **not** recommend, so the filtering logic is visible, not just the results.

6. Append a log entry to `research/wiki/log.md`: `## [YYYY-MM-DD] recommend | [N] files reviewed, [M] ripple candidates found` with a one-line summary.

7. Post a brief summary to the conversation: how many files reviewed, how many ripple candidates found (name them), and a pointer to the recommendations file. If there's nothing new to review, say so plainly and stop — don't pad the output. End with `<run-summary>one or two sentences on what was found</run-summary>`.

Never invent facts, never write to `research/wiki/` directly, never delete or edit files in `research/raw/` or `research/inbox/` — recommend only.

## Running this as a scheduled task

Save this file under your scheduled-tasks directory alongside a copy of (or path to) `SCHEMA.md`, then create the scheduled task pointing at it. Weekly is a reasonable default — matched to how often your upstream sources actually produce new material.
