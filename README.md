# LLM Wiki — Schema & Maintenance Skills

A schema and two Claude Cowork scheduled-task templates for running a personal "second brain" wiki that an AI assistant maintains for you, rather than one you maintain by hand.

This isn't a wiki app. It's a folder convention plus an instruction file ([`SCHEMA.md`](SCHEMA.md)) that tells an assistant how to read, write, and maintain a `research/` folder of markdown pages — and two periodic tasks that keep it honest over time instead of slowly rotting.

## The idea

Three things compete for the same kind of note, and conflating them is what makes most personal knowledge bases either bloat with noise or ossify:

| System | Answers | Timescale |
|---|---|---|
| A day-to-day notes/task file | "What's live right now?" | Days–weeks, trimmed |
| A project's own log | "What happened on this specific project?" | Life of the project |
| The wiki | "What have I learned that's still true regardless of which project it came from?" | Permanent, compounding |

`SCHEMA.md` defines the wiki's page types (concept, entity, source-summary, synthesis, doctrine), the operations an assistant can perform on it (ingest, query, lint, recommend), and — the part that actually keeps it from becoming a dumping ground — an explicit durability test for what's allowed in.

## What's in this repo

- [`SCHEMA.md`](SCHEMA.md) — the wiki convention itself. Read by an assistant at the start of any session touching `research/`.
- [`skills/wiki-lint/`](skills/wiki-lint/SKILL.md) — periodic maintenance pass: contradictions, stale claims, orphan pages, broken links, data gaps. Auto-fixes only the mechanical, judgment-free issues; everything else goes in a report for you to decide.
- [`skills/wiki-ripple-recommender/`](skills/wiki-ripple-recommender/SKILL.md) — periodic scan of newly captured raw material, recommending what's durable enough to promote into the wiki. Recommendations only — it never writes wiki pages itself.

## Setup

**Quickest way:** point an AI assistant at this repo and ask it to set it up for you in your own `research/` folder — it can walk you through filling in the ✏️ sections in `SCHEMA.md` and the two skill files, and register the scheduled tasks.

**Manual way:**

1. Create a `research/` folder with `raw/`, `inbox/`, and `wiki/` subfolders (see `SCHEMA.md` for the full layout), and copy `SCHEMA.md` in at the top level.
2. Fill in `SCHEMA.md`'s **✏️ Current territories** section once you've used it for a while — leave it as the placeholder to start.
3. Copy each skill's `SKILL.md` into its own folder under your scheduled-tasks directory, fill in its ✏️ sections, and create the two scheduled tasks (lint monthly, recommend weekly are reasonable defaults).

## Using this with other AI assistants

Nothing here is Claude-specific — `SCHEMA.md` and both skill files are plain instructions for whatever assistant you're using. To set this up with ChatGPT or another assistant, give it this repo's URL (or paste in the files) and ask it to get it working using that assistant's own scheduling mechanism.

## License

MIT — see [LICENSE](LICENSE).
