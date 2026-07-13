---
name: ai-retro-brief
description: Use when the user asks to synthesize, summarize, or analyze the AI session retro log — recurring friction in AI-assisted work, patterns across AI sessions, or prep for a team retrospective (e.g. "what patterns are in our AI retros", "prep the AI retro summary for Friday"). Companion to ai-session-retro, which writes the entries this skill reads.
metadata:
  version: "1.2"
---

# AI Retro Brief

Read the team's AI session retro entries and surface recurring patterns as an actionable one-page brief for their retrospective. The audience is humans walking into a retro — they need ranked patterns, evidence, and proposed actions, not a restatement of every entry.

## Input

Read every file in `docs/ai-retros/entries/` (sorted by the date in the filename). Each entry carries a `**Skill:**` version stamp — parse each according to its stamp and note any resulting gaps rather than guessing. Legacy format: if a single `docs/ai-retros/retro-log.md` exists (v1.1 wrote one append-only file, entries under `## YYYY-MM-DD — summary` headings), parse its entries too.

If there are fewer than 3 entries, say so and give only a light summary — pattern claims need repetition to mean anything.

**Multi-repo rollup:** if asked for patterns across several repos, read each repo's `docs/ai-retros/entries/` and attribute every theme to its repo(s) in the brief.

## The brief

Exactly this structure:

```markdown
# AI Collaboration Retro Brief — <date range>, <N> sessions

## Recurring friction
<Themes ranked by frequency × cost. Each theme: how many sessions, representative examples, root-cause label, the fix entries proposed.>

## Root-cause distribution
<Counts per root-cause label, then group them: briefing (ambiguous-instruction, missing-context, incorrect-context, changed-requirements) vs. documentation (missing-documentation, incorrect-documentation) vs. codebase (codebase-friction) vs. tooling (missing-access-or-tool, environment-friction) vs. agent (agent-error). State plainly which group dominates — that tells the team whether the fix is process, docs, code, tooling, or model.>

## Were suggested fixes adopted?
<For each past "Do this first": did the same friction stop appearing afterward? Unadopted repeat suggestions deserve the team's attention most.>

## Top 3 recommended actions
<Each tied to specific evidence from specific entries. Concrete enough to become a retro action item — owner-assignable, checkable.>

## Trend
<Is friction per session going up, down, or flat? One paragraph, honest about small-sample uncertainty.>
```

## Rules

- Every claim traces to specific dated entries. No pattern claims from a single occurrence.
- Do not soften findings: if most friction traces to briefing quality, say that; if most traces to agent error, say that too.
- Root-cause counts use the fixed vocabulary as-is; if an entry contains an out-of-vocabulary label, flag it rather than silently reclassifying.
- Exclude backfilled entries from the "were fixes adopted?" analysis (they weren't written in sequence), but include their friction in counts.
- Keep it to one page. The brief feeds a conversation; it doesn't replace one.

Save as `docs/ai-retros/briefs/brief-YYYY-MM-DD.md` (dated, so successive briefs accumulate), stamp it with `ai-retro-brief v<version from this file's frontmatter>`, commit it, and present it to the user.
