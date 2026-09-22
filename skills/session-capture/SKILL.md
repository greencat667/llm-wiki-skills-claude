---
name: session-capture
description: Periodic archive of new chat sessions to research/raw/sessions/ as portable markdown, so other AI tools (or a future model) can read them
---

Turn recent chat sessions into portable, plain-text markdown captures in `research/raw/sessions/`, so that other AI tools (a future local model, a different assistant, a fresh session with no memory of this one) can read them. Each run is fresh with no memory of previous runs, so follow these steps exactly.

This file is a template. Fill in the ✏️ section below, and see the note on the Claude-specific tool dependency before using it elsewhere.

## ✏️ Your capture folder

Default: `research/raw/sessions/` (matching [`SCHEMA.md`](../../SCHEMA.md)'s directory convention — use a different path if your setup differs).

These captures are **immutable sources**. Only ever create new files here. Never edit or delete existing ones.

## A dependency worth knowing about

This skill needs a way to (a) list your recent chat sessions and (b) read a given session's transcript. On Claude Cowork/Claude Code, that's the `list_sessions` / `read_transcript` tools. If you're running this on another assistant, you'll need whatever plays the equivalent role there — an exported chat history, a sessions API, or (failing that) manually pasting in the transcripts you want captured. Without something in that role, this skill has nothing to read.

## Steps

1. List your recent sessions (on Claude, `list_sessions`, limit 100 — use a high limit, not a low one; a growing capture folder makes it easy to silently miss older uncaptured sessions if the limit is too tight).

2. List existing capture files in your capture folder (`ls` or a glob). Each existing capture's filename ends with the first 8 characters of the session id it came from (e.g. `2026-07-14-weekly-grant-scan-6eedaccd.md`). Build the set of session-id prefixes already captured.

3. For each session from step 1 that is **not** already captured and is **not** currently running (skip the session you're running inside, and skip anything still marked "running"):
   a. Read its transcript (on Claude, `read_transcript`, `max_wait_seconds = 0`, a limit high enough to get the substance — 60 is a reasonable default). If the transcript is trivial or empty (a couple of messages, no real work), skip it — don't create a file for noise.
   b. Write a new file named `<YYYY-MM-DD>-<short-kebab-slug-from-title>-<first8ofSessionId>.md`, dated from the session's most recent activity.
   c. Use this frontmatter and structure:
      ```yaml
      ---
      title: <session title>
      type: session-capture
      source-type: conversation
      session-id: <full session id>
      date: <YYYY-MM-DD>
      participants: [<you>, <assistant name>]
      tags: []
      ---
      ```
      ```markdown
      # Session capture — <date> — <title>

      *Immutable record. Never edited after creation. Durable insight is rippled into the wiki separately, not here.*

      ## What was asked
      <1–3 sentences>

      ## What was done
      <compressed bullets — the substance, not a full transcript>

      ## Key findings / decisions (and why)
      <the durable, least-recoverable thinking — this is the point of the capture>

      ## Open threads
      <anything unfinished>
      ```
   Keep each capture compressed but faithful: preserve decisions and the reasoning behind them, drop filler.

4. Do **not** ingest anything into `research/wiki/` yourself. Capture is automated; deciding what durable insight enters the wiki stays a human choice — [`wiki-ripple-recommender`](../wiki-ripple-recommender/SKILL.md) surfaces recommendations from what lands here, and you (or a session) do the actual ingest. You only write to the capture folder.

5. At the end, post a one-line summary: how many sessions were captured and their titles (or "no new sessions to capture"). If anything captured has clearly durable insight worth rippling into the wiki, name those files so it's easy to choose what to ingest later.

Follow [`SCHEMA.md`](../../SCHEMA.md)'s spirit: raw sources are immutable, provenance is explicit, never invent facts.

## Running this as a scheduled task

Save this file under your scheduled-tasks directory, then create the scheduled task pointing at it. Daily matches how fast a typical chat-session backlog grows; a lower-traffic setup could run this weekly instead.
