---
name: teamretro-post-recommendations
description: Use when the user wants the AI's retro recommendations prepared and posted into TeamRetro — "post the AI's recommendations to TeamRetro", "prep our TeamRetro retro with the AI's findings", "park the top fixes for next retro", "add the AI's feedback to Friday's retro". Do NOT use for general TeamRetro administration (creating retros, health checks, teams, or managing existing items), and not for a tool-agnostic summary the user brings to their retro themselves — that's ai-retro-brief.
metadata:
  version: "0.2"
---

# Post AI Recommendations to TeamRetro

The TeamRetro-native counterpart of `ai-retro-brief`. Where that skill prepares a summary the user brings to their retro however they run it, this one prepares the AI's recommendations **and posts them into TeamRetro** — so the AI's ideas land on the board next to everyone else's.

This is the AI-participates-in-the-retro flow. The AI's voice stays visible: every posted item is prefixed `[AI retro]` so a human reading the board knows a teammate's agent raised it, not a person. The human decides what gets posted and where; the AI never posts on its own.

## Prerequisite — TeamRetro MCP connection

Requires an authenticated TeamRetro MCP connection (the server exposing `create_parked_item`, `create_action`, `create_retrospective_idea`, `list_teams`, `list_retrospectives`, `get_retrospective`). If those tools aren't present, say so plainly and stop:

> No TeamRetro MCP connection is available in this session, so I can't post anything. You can still get the same analysis as a document with the `ai-retro-brief` skill and bring it to your retro yourself.

Never fabricate a posting (no invented IDs, no "posted!" without a real tool response).

## Step 1 — Prepare the recommendations

Synthesize recommendations from the AI session retro log exactly as `ai-retro-brief` does — read `docs/ai-retros/entries/` (fewer than 3 entries: say so, recommend logging more sessions first, and only proceed if the user insists), rank recurring friction by frequency × cost, and derive the top recommended actions, each tied to specific dated entries. Follow that skill's rules: evidence-cited, unsoftened, fixed vocabulary as-is.

Save the analysis as a brief (`docs/ai-retros/briefs/brief-YYYY-MM-DD.md`, per `ai-retro-brief`'s template, stamped `teamretro-post-recommendations v0.2`) and commit it — the brief is the record of what was recommended; the postings reference it. If the user points at an existing committed brief or entry instead, use its recommendations ("Top 3 recommended actions" / "Do this first") rather than re-deriving.

**Idempotency — skip already-posted recommendations.** Before proposing anything, scan the source brief/entry for existing `Posted:` refs (see Step 4). Skip those and say so. If everything is already posted, stop.

## Step 2 — Choose the destination with the user

Show the user the prepared recommendations, then ask where they should go if it isn't obvious:

- **Parked item** (`create_parked_item`) — the default. A parked item is a discussion topic queued for the team's next retro with no commitment to act yet, which is exactly what a recommendation is. Needs a team: resolve via `list_teams` when the user names one; confirm the matched team (name + `url`) before writing.
- **Action** (`create_action`) — only when a recommendation has a clear owner. Actions imply commitment; don't manufacture one. Set `assignedTo` only if the user names the owner's email; `priority`/`due` only if asked.
- **Retrospective ideas** (`create_retrospective_idea`) — when the user wants the recommendations contributed into a specific retro. Resolve the target with the user, don't guess: `list_teams` → `list_retrospectives` (filter `teamIds`, sort `-date`) → the user picks. Optionally `get_retrospective` with `expand=groups` to place ideas in a chosen column via `topicId` (otherwise they land in the first column). Real tool behavior to relay honestly:
  - If a retro is **open**, the idea lands on the live board; if none is open, it becomes a parked item — the response's `createdAs` field (`"idea"` vs `"parkedItem"`) says which. Report it: "no retro was open, so it was parked for next time."
  - More than one open retro → 409 with candidates; pass the user-chosen `retrospectiveId` only to disambiguate.
  - Idea creation needs a **user** token, not an API key; on that rejection, report it and offer parked items instead.

## Step 3 — Consent per write, then post

Compose each item: `[AI retro]` prefix + the recommendation, tight enough to read on a card, with the source date. Example:

`[AI retro] Name the current notification pipeline in CLAUDE.md — removes a repeated wrong-pipeline detour (from 2026-07-13 brief)`

Never include secrets, credentials, tokens, or customer data; if a recommendation can't be stated without sensitive detail, don't post it and say why.

Never auto-post. Show the user the exact batch — full text of every item, item type, target team (name + url) and retro if applicable, any assignee — and get explicit confirmation. If they edit wording, post the edited text; if they drop an item, don't post it. Only then call the tool, once per item.

## Step 4 — Record the postings and commit

After each successful write, record the returned identifier under the recommendation in the source brief/entry:

```
Posted: <item type> <id or url> (YYYY-MM-DD)
```

Note when an intended idea landed as a parked item (`createdAs`), e.g. `Posted: parked-item <id> (2026-07-13) — no retro open, parked`. Use the id/url the tool actually returned — never a placeholder. Commit the change (`docs: record TeamRetro posting refs`). This writeback is the idempotency guard Step 1 reads; without the commit, the next run re-posts.

## Hard rules (non-negotiable)

- Requires an authenticated TeamRetro MCP connection; if absent, point to `ai-retro-brief` and stop. The base practice never requires TeamRetro.
- Consent per write: show the exact text, target, and item type; get explicit confirmation before each batch. Never auto-post.
- Post only recommendations the user has just reviewed, or from a committed brief/entry — never unreviewed drafts.
- Idempotency: skip recommendations already carrying a `Posted:` ref; write refs back and commit after posting.
- Never create retrospectives, health checks, or teams. Never modify or delete anything in TeamRetro. Parked items, actions, and ideas only — forward writes only.
- Attribution: every posted title is prefixed `[AI retro]` — the agent's voice is visible, never disguised as a human's.
