---
name: ai-session-retro
description: Use when a working session with an AI is wrapping up and should be reviewed and logged — "run a retro on this session", "AI session retro", "debrief this session", "log this session" — or when asked to retro a past session from a transcript. Offer it proactively at the natural end of a large or multi-step session. Do NOT use this to create or facilitate a retrospective in a retrospective product (TeamRetro or similar) — this skill reviews AI working sessions only.
metadata:
  version: "1.2"
---

# AI Session Retro

At the end of a working session, produce an honest, evidence-based retrospective of that session and save it to a persistent per-repo log. Over time these entries accumulate into a dataset the team can analyze (see the companion `ai-retro-brief` skill).

One session's friction is noise; the same friction across eight sessions is a process problem. The log is only useful if entries are consistent (identical schema, so they're comparable) and honest (specific, evidence-cited observations, not vibes). Treat vagueness as a failure mode.

## The log

One file per entry: `docs/ai-retros/entries/YYYY-MM-DD-<short-slug>.md`, relative to the repo root. Create the directories if needed. Never edit or delete existing entries. If the slug would collide with an existing file, append `-2`.

Commit the new entry as its own commit (`docs: add AI session retro entry`). If you're on a feature branch or worktree, that's fine — the entry merges with the work it describes. If this isn't a git repo, ask the user where the team's shared log should live; an entry written to a scratch directory dies with the session, which defeats the purpose.

## Step 1 — Review the actual session

Walk back through the conversation and collect concrete moments: instructions that were ambiguous and forced a guess, context or access that was missing, code or docs that made the work slower than it should have been, work redone after a correction, tool failures, and things that made the work fast. Every observation must trace to one of these moments — if you can't cite a moment, drop the observation.

**Retroactive mode:** if given a transcript of a past session, treat the transcript as the session. Date the entry by the session's date (not today), add `**Backfilled:** yes` to the header, and skip Step 4's loop check. When backfilling several transcripts, process oldest first.

## Step 2 — Honesty rules

- No praise, no filler. If a section has nothing real, write "Nothing notable" — a valid and useful answer.
- Critiquing the human's inputs (unclear briefs, missing context, late requirements) is expected — that's usually where the actionable material lives. Describe the effect, not a judgment of the person.
- Critique yourself too: wrong assumptions, errors, wasted time. You won't notice all your own errors — say so when uncertain.
- Never quote secrets, credentials, tokens, or customer data in an entry. Entries are committed to the repo — describe the friction without reproducing sensitive content.
- Label every friction item with exactly one root cause from this fixed list (a fixed vocabulary is what makes later analysis possible — never invent new labels; pick the closest fit):
  - `ambiguous-instruction` — the ask left a key parameter open (audience, scope, definition, time window) and a guess was forced
  - `missing-context` — a standing fact the team knows but didn't share (conventions, taxonomy, history)
  - `incorrect-context` — information provided was wrong (bad data, wrong figures, mistaken claims)
  - `missing-documentation` — a doc that should exist doesn't (no README, no CLAUDE.md guidance, no spec, no runbook)
  - `incorrect-documentation` — a doc exists but is stale or wrong, and was relied on
  - `codebase-friction` — the code itself made the work slow or error-prone: tech debt, inconsistent patterns, duplicated approaches, confusing structure
  - `missing-access-or-tool` — a needed connector, permission, or tool wasn't available
  - `agent-error` — the agent's own mistake: wrong assumption, stale knowledge, bug it introduced
  - `changed-requirements` — the ask changed mid-session and work was redone
  - `environment-friction` — tooling failures, timeouts, sandbox or platform issues

## Step 3 — Write the entry

Use exactly this template (the strict format is what lets `ai-retro-brief` parse it later — don't improvise sections). Every friction item ends with a concrete proposed fix — sized so it could become a ticket. No finding without a fix.

```markdown
# YYYY-MM-DD — <one-line task summary>

**Session size:** <rough scale: e.g. "~40 turns, 3 deliverables">
**Outcome:** <shipped / partially shipped / blocked>
**Skill:** ai-session-retro v<version from this file's frontmatter>

## Went well
- <max 3 items, each with evidence from the session>

## Friction
- **[root-cause]** <what happened, when, and what it cost (redone work, wasted time, wrong output)>
  → Fix: <concrete next step, ticket-sized>

## Guesses made
- <decision taken without enough information + the assumption behind it; flag any still-unverified>

## Do this first
<the single highest-leverage fix from above, with its expected payoff in one line>

## Also worth doing
- <at most 2 more, or omit this section — a long wishlist prioritizes nothing>
```

## Step 4 — Close the loop

Read the most recent existing entry (by filename date) and check its "Do this first". If this session shows it was adopted — or the same friction recurred anyway — note that in one line at the end of the new entry. This turns the log from a diary into a feedback loop. (Older entries may use a `## One change` section from v1.1 — read that instead; parse any entry according to its `**Skill:**` version stamp.)

## Step 5 — Confirm, then optionally file it

Show the user the full entry in chat — they were in the session too, and their correction is part of the record. If they correct something, update the file before committing.

After the user confirms: if this session has MCP tools for filing team work or retro items (for example a TeamRetro server exposing `create_parked_item` / `create_action`), offer **once** to file the "Do this first" fix — as an action if it has a clear owner, otherwise as a parked item for the team's next retro. Include the fix, its root-cause label, and the entry date. If the user accepts, record the returned item ID or URL in the entry as `Filed: <ref>` so re-runs don't double-file. Never file automatically, never file more than the confirmed fix, and never create retrospectives or retrospective ideas in any product from this skill. If no such tools exist or the user declines, move on without comment.

If there are 5+ entries since the last brief (or no brief exists), mention once that the `ai-retro-brief` skill can synthesize the log for the team's next retrospective. Don't nag beyond that.
