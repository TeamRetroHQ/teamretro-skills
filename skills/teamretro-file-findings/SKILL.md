---
name: teamretro-file-findings
description: Use when the human wants confirmed AI-retro findings filed into TeamRetro — "file the top fix into TeamRetro", "park this for next retro", "add the AI's reflections to Friday's retro", "put these findings on the board". Files a "Do this first" fix or a brief's recommended actions as a parked item or action, or contributes selected log findings as ideas into a retro the user picks. Do NOT use for general TeamRetro administration (creating retros, health checks, teams, or managing existing items) — this skill only files AI-retro findings the human has already sanity-checked.
metadata:
  version: "0.1"
---

# File Findings to TeamRetro

The `ai-session-retro` and `ai-retro-brief` skills produce findings the AI is confident enough to raise. This skill carries the ones a human has already sanity-checked into TeamRetro — as backlog input for the next retro, or as the AI's own contribution to a retro that's running. The human decides what goes where; the AI never files on its own.

This is the AI-participates-in-the-retro flow. The AI's voice stays visible: every filed item is prefixed `[AI retro]` so a human reading the board knows a teammate's agent raised it, not a person.

## Prerequisite — TeamRetro MCP connection

This skill requires an authenticated TeamRetro MCP connection (the server exposing `create_parked_item`, `create_action`, `create_retrospective_idea`, `list_teams`, `list_retrospectives`, `get_retrospective`). If those tools aren't present in the session, say so plainly and stop:

> No TeamRetro MCP connection is available in this session, so I can't file anything. The base practice doesn't need it — `ai-session-retro` has a built-in one-off filing step, and you can copy any finding into TeamRetro by hand.

Never require TeamRetro for the base practice, and never fabricate a filing (no invented IDs, no "filed!" without a real tool response).

## What you may file — and only this

Only findings a human has already sanity-checked, meaning they come from **committed** entries in `docs/ai-retros/entries/` or a committed brief in `docs/ai-retros/briefs/`. Never file from an unreviewed draft, from chat, or from an entry you just wrote but haven't shown the user and committed.

Never create retrospectives, health checks, teams, agreements, or estimations. Never update or delete anything already in TeamRetro. This skill writes exactly three item types, and only forward: parked items, actions, and retrospective ideas.

## Step 1 — Read the source and pick candidates

Read the entry or brief the user pointed at (or, if they didn't, the most recent committed entry). Extract the fileable findings:

- From an **entry**: the `## Do this first` fix (the default single candidate), and any `## Also worth doing` items if the user asks for them.
- From a **brief**: the `## Top 3 recommended actions`.

**Idempotency — skip already-filed findings.** Before proposing anything, scan the source file for existing `Filed:` references (see Step 5). A finding already marked `Filed:` has been filed — drop it from the candidate list and tell the user you skipped it. If every candidate is already filed, say so and stop.

## Step 2 — Choose the destination with the user

Two modes. Ask which the user wants if it isn't obvious from their request.

### Mode A — File to the backlog (default for a fix)

For a "Do this first" fix or a brief's recommended actions with no live retro in play, file each as:

- a **parked item** (`create_parked_item`) — the default. A parked item is a discussion topic queued for the team's next retro with no commitment to act yet, which is exactly what retro input is.
- an **action** (`create_action`) — only when the finding has a clear owner. Actions carry an assignee and imply a commitment; don't manufacture one. Set `assignedTo` only if the user names the owner's email, and `priority`/`due` only if they ask.

Both need a team. Resolve it with `list_teams` when the user names a team; confirm the matched team (show its name and `url`) before writing.

### Mode B — Contribute to a retrospective (the AI participates)

When the team has a retro running and the user asks to add the AI's reflections to it, file selected findings as retrospective **ideas** (`create_retrospective_idea`) into the retro the **user picks**.

Resolve the target retro, don't guess it:

1. `list_teams` → team id (if the user named a team).
2. `list_retrospectives` (filter `teamIds`, sort `-date`) to show the user recent/candidate retros; let them pick one. Never assume which retro "Friday's retro" means — confirm the specific one.
3. Optionally `get_retrospective` with `expand=groups` to read the columns; pass a chosen column as `topicId` if the user wants the ideas in a specific column (otherwise they land in the first column).

How `create_retrospective_idea` behaves (design to the real tool, tell the user what it does):

- If a retro is **open** for the team, the idea is added to the live board. If **no** retro is open, the tool saves it as a parked item instead — its response field `createdAs` (`"idea"` or `"parkedItem"`) tells you which happened. Relay that honestly: "no retro was open, so it was parked for next time."
- If the team has **more than one** retro open, the call returns 409 with the candidates; pass the user-chosen `retrospectiveId` to disambiguate. Only pass `retrospectiveId` in that case.
- Creating an idea needs a **user** token, not an API key. If the tool rejects the call for that reason, report it and offer Mode A (park it) instead.

## Step 3 — Compose the filed text

Prefix every item's `title` with `[AI retro]` so the source is visible on the board. Keep it to the finding itself — the fix or action text, tight enough to read on a card. Append the source entry/brief date so a human can trace it back. Example:

`[AI retro] Name the current notification pipeline in CLAUDE.md — removes a repeated wrong-pipeline detour (from 2026-07-13 session retro)`

Never put secrets, credentials, tokens, or customer data in filed text — the same rule the entries follow. If a finding can't be stated without sensitive detail, don't file it; tell the user why.

## Step 4 — Consent per write, then file

Never auto-file. For each batch, show the user **exactly** what will be written before calling any tool:

- the full `title` text (with the `[AI retro]` prefix),
- the item type (parked item / action / idea),
- the target team (name + url) and, in Mode B, the target retro,
- for actions, any assignee/priority/due.

Get explicit confirmation for the batch. Only after the user says yes, call the tool once per item. If the user edits the wording, file the edited text. If they drop an item, don't file it.

## Step 5 — Write back the Filed ref and commit

After each successful write, record the returned identifier in the source file so a re-run can't double-file. Immediately under the finding it came from, add:

```
Filed: <item type> <id or url> (YYYY-MM-DD)
```

For example: `Filed: parked-item aB3...xY (2026-07-13)` or `Filed: idea <url> (2026-07-13)`. Use the id and/or url the tool actually returned — never a placeholder. Note whether an intended idea landed as a parked item (`createdAs`), e.g. `Filed: parked-item <id> (2026-07-13) — no retro open, parked`.

Commit the source-file change (`docs: record TeamRetro filing ref`). This writeback is the idempotency guard Step 1 reads; without the commit, the next run re-files.

## Hard rules (non-negotiable)

- Requires an authenticated TeamRetro MCP connection; if absent, point to the `ai-session-retro` built-in fallback and stop. Never require TeamRetro for the base practice.
- Consent per write: show the exact text, target, and item type; get explicit confirmation before each batch. Never auto-file.
- Only file human-sanity-checked findings from committed entries/briefs — never an unreviewed draft.
- Idempotency: skip findings that already carry a `Filed:` ref; after filing, write the ref back and commit.
- Never create retrospectives, health checks, or teams. Never modify or delete anything in TeamRetro. Parked items, actions, and ideas only — forward writes only.
- Attribution: every filed title is prefixed `[AI retro]` — the agent's voice is visible, never disguised as a human's.
