---
name: ai-session-retro
description: Use when a working session with an AI is wrapping up and should be reviewed and logged — "run a retro on this session", "AI session retro", "debrief this session", "log this session" — or when asked to retro a past session from a transcript. Offer it proactively at the natural end of a large or multi-step session. Do NOT use this to create or facilitate a retrospective in a retrospective product (TeamRetro or similar) — this skill reviews AI working sessions only.
metadata:
  version: "2.1"
---

# AI Session Retro

At the end of a working session, produce an honest, evidence-based retrospective of that session and save it to a persistent log. Over time these entries accumulate into a dataset the team can analyze (see the companion `ai-retro-brief` skill).

One session's friction is noise; the same friction across eight sessions is a process problem. The log is only useful if entries are consistent (identical schema, so they're comparable) and honest (specific, evidence-cited observations, not vibes). Treat vagueness as a failure mode.

The root-cause vocabulary below is a synchronized copy; its canonical definition and change process live in the team's engineering docs (docs-first). Never alter labels ad hoc — vocabulary changes happen at a periodic review and arrive here as a version bump.

Skill invocation is probabilistic — capture fails silently when nothing triggers it. See [SETUP.md](SETUP.md) for hook, command, and convention snippets that make the trigger deterministic.

## The log

One file per entry: `docs/ai-retros/entries/YYYY-MM-DD-<short-slug>.md`, relative to the repo root. Create the directories if needed. Never edit or delete existing entries. If the slug would collide with an existing file, append `-2`.

The log lives at the work's **review scope** — the boundary where fixes land. For most dev work that's the repo: its docs, config, and conventions are repo-homed, so its friction log is too. For non-code work (an ads account, a support inbox, a client engagement), keep one log per scope in that workspace's document home. If there's no natural home, ask the user where the team's shared log should live — an entry written to a scratch directory dies with the session, which defeats the purpose.

Commit the new entry as its own commit (`docs: add AI session retro entry`). If you're on a feature branch or worktree, that's fine — the entry merges with the work it describes.

## Step 1 — Review the actual session

Walk back through the conversation and collect concrete moments: instructions that were ambiguous and forced a guess, context or access that was missing, code, structures, or docs that made the work slower than it should have been, work redone after a correction, tool failures, and things that made the work fast. Every observation must trace to one of these moments — if you can't cite a moment, drop the observation.

**Retroactive mode:** if given a transcript of a past session, treat the transcript as the session. Date the entry by the session's date (not today), add `**Backfilled:** yes` to the header, and skip Step 4's loop check. When backfilling several transcripts, process oldest first.

## Step 2 — Honesty rules

- No praise, no filler. If a section has nothing real, write "Nothing notable" — a valid and useful answer.
- Critiquing the human's inputs (unclear briefs, missing context, late requirements) is expected — that's usually where the actionable material lives. Describe the effect, never the person (see the never-log list).
- Critique yourself too: wrong assumptions, errors, wasted time. You won't notice all your own errors — say so when uncertain.
- **Never put in an entry** (entries are committed, shared, and outlive their context):
  - secrets, credentials, tokens, or API keys — in any form, even partial;
  - customer data or personal information;
  - **names, roles, or anything that identifies a person** — describe the gap and what it cost, never who caused it ("the brief left the audience open," not "X's brief"); friction items critique inputs and systems, not people;
  - unreleased business numbers or confidential figures — describe the effect, not the data.
  If a scope's friction can't be described without sensitive context, keep that scope's entries private and share only the aggregated brief.
- Label every friction item with exactly one root cause from this fixed vocabulary. Never invent labels; pick the closest fit, and if the fit is strained, say so in the evidence — strained fits are input to the team's vocabulary review:
  - `ambiguous-instruction` — the ask left a key parameter open (audience, scope, definition, time window) and a guess was forced
  - `missing-context` — a standing fact the team knows but didn't share (conventions, taxonomy, history)
  - `incorrect-context` — information provided was wrong (bad data, wrong figures, mistaken claims)
  - `missing-documentation` — a doc that should exist doesn't (no README or context file, no spec, no runbook/SOP)
  - `incorrect-documentation` — a doc exists but is stale or wrong, and was relied on
  - `work-material-friction` — the material being worked on made the work slow or error-prone: tech debt, inconsistent patterns, or confusing structure in code; a tangled account, board, spreadsheet, or template outside it *(v1.x label: `codebase-friction`; always give the concrete material in the evidence)*
  - `missing-access-or-tool` — a needed connector, permission, or tool wasn't available
  - `agent-error` — the agent's own mistake: wrong assumption, stale knowledge, bug it introduced
  - `changed-requirements` — the ask changed mid-session and work was redone
  - `environment-friction` — tooling failures, timeouts, sandbox or platform issues (includes subagent/orchestration failures, for now)
- Boundary rules for the common collisions:
  - Gap in *this ask* → `ambiguous-instruction`; gap in *standing knowledge* → `missing-context`. If clarifying revealed what was always meant, it was ambiguity; if the goal genuinely moved, it's `changed-requirements`.
  - The agent-knowable test: was the fact *findable anywhere you could reasonably look* (repo, docs, workspace)? Found nowhere it should have been → `missing-documentation`; supplied only later, by a person → `missing-context`.
  - Wrong thing *said* → `incorrect-context`; wrong thing *written* and relied on → `incorrect-documentation`.
  - Tool never available → `missing-access-or-tool`; tool existed and misbehaved → `environment-friction`.
  - Material fine but unexplained → `missing-documentation`; the material itself the drag → `work-material-friction`.
  - `agent-error` is the residual: use it only when the inputs were adequate and the agent still erred. If the error traces to a bad input, label the input.

## Step 3 — Write the entry

Use exactly this template (the strict format is what lets `ai-retro-brief` parse it later — don't improvise sections). Every friction item ends with a concrete proposed fix — sized so it could become a ticket — and names the **altitude** the fix targets: `memory` (private note) · `skill` (procedure/prompt) · `env` (config/access) · `docs` · `material` (the code/account/template itself) · `process` (how work is briefed or run) · `upstream` (vendor/product). The cause points at the altitude: briefing friction → process or memory; documentation → docs; work material → material; tooling → env or upstream; agent → skill or guardrail. Mark cost estimates you're unsure of with `(est.)`.

```markdown
# YYYY-MM-DD — <one-line task summary>

**Session size:** <rough scale: e.g. "~40 turns, 3 deliverables">
**Outcome:** <shipped / partially shipped / blocked>
**Skill:** ai-session-retro v<version from this file's frontmatter>

## Went well
- <max 3 items, each with evidence from the session>

## Friction
- **[root-cause]** <what happened, when, and what it cost (redone work, wasted time (est.), wrong output)>
  → Fix (altitude: <memory|skill|env|docs|material|process|upstream>): <concrete next step, ticket-sized>

## Guesses made
- <decision taken without enough information + the assumption behind it; flag any still-unverified>

## Do this first
<the single highest-leverage fix from above, with its expected payoff in one line>

## Also worth doing
- <at most 2 more, or omit this section — a long wishlist prioritizes nothing>
```

## Step 4 — Close the loop

Read the most recent existing entry (by filename date) and check its "Do this first". If this session shows it was adopted — or the same friction recurred anyway — note that in one line at the end of the new entry. This turns the log from a diary into a feedback loop. (Entries from older skill versions may differ — v1.1 used a `## One change` section, v1.x fix lines carry no altitude tag; parse any entry according to its `**Skill:**` version stamp.)

## Step 5 — Commit, show, then optionally file it

Commit the entry, then show the user the full text. Reporting is deliberately ungated — an agent that needs permission to record friction under-reports — and the user's feedback is additive: they were in the session too, and their correction is part of the record. If they correct something, apply it as a follow-up commit.

Confirmation is reserved for outward actions. If this session has MCP tools for filing team work or retro items (for example a TeamRetro server exposing `create_parked_item` / `create_action`), offer **once** to file the "Do this first" fix — as an action if it has a clear owner, otherwise as a parked item for the team's next retro. Include the fix, its root-cause label, its altitude, and the entry date. If the user accepts, record the returned item ID or URL in the entry as `Filed: <ref>` so re-runs don't double-file. Never file automatically, never file more than the confirmed fix, and never create retrospectives or retrospective ideas in any product from this skill. If no such tools exist or the user declines, move on without comment.

If there are 5+ entries since the last brief (or no brief exists), mention once that the `ai-retro-brief` skill can synthesize the log for the team's next retrospective. Don't nag beyond that.

## Why this shape (provenance)

The design choices above aren't arbitrary; the load-bearing ones and their sources:

- **Evidence-cited entries with root-cause labels** operationalize *error analysis* — "the most important activity in evals" ([Husain & Shankar, evals FAQ](https://hamel.dev/blog/posts/evals-faq/)). Fixed vocabularies can be labeled reliably (MAST reached κ = 0.88 on 14 failure modes — [arXiv 2503.13657](https://arxiv.org/abs/2503.13657)); free-form categories drift per person and stop aggregating.
- **Harvesting learnings back into team artifacts** is Rahul Garg's *Feedback Flywheel* pattern ([martinfowler.com](https://martinfowler.com/articles/reduce-friction-ai/feedback-flywheel.html)); this skill is the capture end of that loop.
- **Human correction as first-class signal**: an 8-week production study found ~70% of silent agent failures were first caught by a person noticing something off — not by unit tests, health checks, or governance audits ([arXiv 2606.14589](https://arxiv.org/abs/2606.14589)). That's why the user's post-commit corrections are part of the record.
- **Ungated reporting, gated reliance**: reports are cheap and reversible; writes agents later *rely on* (memory, context files, config) are a poisoning-and-drift surface (OWASP Agentic Top 10 [ASI06](https://genai.owasp.org/); drift from accumulated benign context: [arXiv 2605.17830](https://arxiv.org/html/2605.17830v1)) and get ordinary review instead.
- **The altitude tag** follows the field's convergent rule — demote ephemeral memory, promote durable learning to versioned artifacts ([OpenAI Codex best practices](https://developers.openai.com/codex/learn/best-practices); [Devin docs](https://docs.devin.ai/desktop/cascade/memories)).
