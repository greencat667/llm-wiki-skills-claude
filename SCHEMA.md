# Research Wiki — Schema

This file tells an AI assistant how to operate your research wiki. It should read this at the start of any session involving `research/`, before doing anything else.

This file is a template. The directory structure, page format, and operations below are generic and work as-is. Fill in your own **✏️ Current territories** at the bottom once you've used it for a while — everything else needs no editing to get started.

---

## Directory structure

```
research/
  raw/          Source documents — immutable. Web clips, PDFs, notes. Never edited by the assistant.
    sessions/   Captured chat-session transcripts — immutable, one file per session (optional).
  inbox/        Un-triaged digests — scheduled tasks and daily-digest-style drops land here.
                Not yet a source, not yet wiki. You decide what gets promoted to raw/ (as a new
                source) or ingested straight into wiki/. The assistant should not ingest from
                inbox/ automatically — it should flag what's there and let you choose.
  wiki/         The maintained knowledge base. The assistant writes and updates this.
    index.md    Catalogue of all wiki pages — read first on every query.
    log.md      Append-only session log — updated after every operation.
  SCHEMA.md     This file.
```

---

## Wiki page format

Every wiki page uses this frontmatter:

```yaml
---
title: 
type: concept | entity | source-summary | synthesis | conversation-log | doctrine
source-type: clipped | conversation | generated   # how this content originated
sources: []       # filenames from raw/ or session description
tags: []
updated: YYYY-MM-DD
---
```

**Page types:**
- **concept** — an idea, framework, or pattern (e.g. "weak signals", "the Brier score")
- **entity** — a person, organisation, tool, or project
- **source-summary** — a summary of a single `raw/` file, with key claims and quotes
- **synthesis** — a cross-source analysis or answer to a specific question; filed when a query produces something worth keeping
- **conversation-log** — key ideas developed in a chat session; generative rather than curated
- **doctrine** — a recurring playbook, method, or set of criteria *you* actually use (e.g. how you
  select projects, how you frame a decision, a set of tests you apply to new tools). Written from
  your own documents and behaviour, not generic advice. Distinct from a `concept` page: a concept
  describes an idea from the outside; a doctrine page describes a method you run, so it should read
  like an operating manual, not an explainer.

**Source types:**
- **clipped** — content from `raw/` (web clips, PDFs, articles)
- **conversation** — ideas developed in dialogue with the assistant; not from an external source
- **generated** — assistant-produced synthesis with no single external source

Conversation-sourced pages should note this clearly at the top: *"Developed in conversation, [date] — not from an external source."* This keeps provenance clear as the wiki grows.

Cross-link liberally using `[[page-name]]` (Obsidian and similar tools render these as backlinks).

---

## Operations

### Ingest

When you drop a file in `raw/` and say "ingest [filename]":

1. Read the raw file in full
2. Briefly discuss key takeaways with you (2–3 bullet points — no walls of text)
3. Write a `source-summary` page in `wiki/`
4. Update or create `concept` and `entity` pages touched by the source
5. Update `wiki/index.md` — add new pages, update the last-updated line
6. Append an entry to `wiki/log.md`: `## [YYYY-MM-DD] ingest | [source title]`

A single source typically touches 3–8 wiki pages. Keep pages tight — a concept page should be a sharp synthesis, not a dump.

### Query

When you ask a question against the wiki:

1. Read `wiki/index.md` to find relevant pages
2. Read those pages in full
3. Synthesise an answer with inline citations to wiki pages and raw sources
4. If the answer is worth keeping, offer to file it as a `synthesis` page
5. Append to `wiki/log.md`: `## [YYYY-MM-DD] query | [question summary]`

### Lint

Run periodically by the [`wiki-lint`](skills/wiki-lint/SKILL.md) scheduled task, or on request. Checks for:
- Contradictions between pages
- Stale claims superseded by newer sources
- Orphan pages (no inbound links from other wiki pages)
- Concepts mentioned but lacking their own page
- Missing cross-references between obviously related pages
- Data gaps that a web search could fill

Report findings as a short list. You decide what to act on.
Append to `wiki/log.md`: `## [YYYY-MM-DD] lint | [brief findings summary]`

### Recommend

Run periodically by the [`wiki-ripple-recommender`](skills/wiki-ripple-recommender/SKILL.md) scheduled task. Surfaces which recent raw captures or inbox items are durable enough to promote into the wiki — recommendations only, never writes wiki pages itself.
Append to `wiki/log.md`: `## [YYYY-MM-DD] recommend | [N] files reviewed, [M] ripple candidates found`

---

## Index conventions

`wiki/index.md` is organised by page type. Each entry: `- [[page-name]] — one-line description`

Update the `Pages:` count and `Last updated:` date on every ingest.

---

## Working style notes

- Work in short bursts. After ingest, give a 2–3-bullet takeaway and stop — don't pre-emptively update every possible page before you've read the summary.
- When uncertain whether a new concept deserves its own page or belongs in an existing one, ask.
- Good synthesis answers should be filed back into the wiki. Offer this at the end of a query response: *"Worth filing this as a synthesis page?"*
- This wiki compounds. Each ingest should visibly connect to what's already there. If a new source contradicts or extends an existing page, say so explicitly.

---

## Memory ↔ wiki boundary

If you also keep a separate `memory/` folder (recent notes, a task list, project logs) alongside this wiki, it's worth being explicit about which system owns what — otherwise the wiki quietly fills up with operational noise that doesn't belong there.

| System | Answers | Timescale |
|---|---|---|
| `memory/` (recent notes, task list, project files) | "What's live right now?" | Days–weeks, trimmed |
| a project's own `log.md` | "What happened on this specific project?" | Life of the project |
| `research/wiki/` | "What have I learned that's still true regardless of which project it came from?" | Permanent, compounding |

**The test:** would this still be useful if you'd forgotten which project or week it came from?
- No → it's operational. It belongs in `memory/` or a project `log.md`, not the wiki.
- Yes → it's durable. It belongs in the wiki, as a `concept`, `entity`, `synthesis`, or `doctrine` page — even if the raw material came from an ordinary work session.

**What this means in practice:**
- A project status update ("found 3 leads this week") stays in the project's `log.md`.
- A pattern the project revealed ("funders are shifting toward X criteria") ripples to the wiki.
- Session captures are a source, not wiki content. They preserve full context; only the durable insight extracted from them gets a wiki page.
- Don't ripple something into the wiki just because a session produced a lot of text. Ripple it because it would still matter to a different future project.

## Project entity pages

A project can have a wiki `entity` page. The rule that keeps this from breaking the memory↔wiki boundary above:

**A project entity page is a pointer, not a status tracker.** It holds: what the project is (one or two sentences), which durable wiki pages it has fed or connects to, and a link back to the real project folder. It does **not** hold current status, next steps, or progress — that stays in your project-tracking system, which remains the single source of truth. If a project page's "status" drifts out of sync with that, it's a sign the page has drifted out of its lane.

**Create one only when a project has actually produced wiki-worthy content** — don't pre-emptively create entity pages for every project folder you have. Most projects will never touch the wiki's territories. The trigger is the same as any other ingest: a project's output feeds one or more durable concept/synthesis/doctrine pages, and it's worth being able to see, from the wiki side, which projects a piece of knowledge traces back to.

## ✏️ Current territories

Replace this with the research areas your wiki actually covers, and keep it updated as it evolves. This is what lets a lint or recommend pass judge what's in-scope vs. operational noise — without it, the durability test above has nothing to check against.

*Example (fill in your own):*
- [Your first research area]
- [Your second research area]
- [Anything explicitly peripheral/adjacent, and why you've admitted it anyway]

---

*Schema template version: 1.0*
