---
name: ai-retro-brief
description: Use when the user asks to synthesize, summarize, or analyze the AI session retro log — recurring friction in AI-assisted work, patterns across AI sessions, or prep for a team retrospective (e.g. "what patterns are in our AI retros", "prep the AI retro summary for Friday"). Companion to ai-session-retro, which writes the entries this skill reads.
metadata:
  version: "2.1"
---

# AI Retro Brief

Read the team's AI session retro entries and surface recurring patterns as an actionable one-page brief for their retrospective. The audience is humans walking into a retro — they need ranked patterns, evidence, and proposed actions, not a restatement of every entry.

The root-cause vocabulary is a synchronized copy of the team's canonical taxonomy (docs-first); never reclassify or rename labels here — flag mismatches for the vocabulary review instead.

## Input

Read every file in `docs/ai-retros/entries/` (sorted by the date in the filename). Each entry carries a `**Skill:**` version stamp — parse each according to its stamp and note any resulting gaps rather than guessing. Version mapping: count v1.x `codebase-friction` entries as `work-material-friction`; v1.x fix lines carry no altitude tag — count them as altitude-unstated; all v2.x entries share the v2.0 format. Legacy format: if a single `docs/ai-retros/retro-log.md` exists (v1.1 wrote one append-only file, entries under `## YYYY-MM-DD — summary` headings), parse its entries too.

If there are fewer than 3 entries, say so and give only a light summary — pattern claims need repetition to mean anything.

**Multi-scope rollup:** if asked for patterns across several review scopes (repos, accounts, workspaces, engagements), read each scope's `docs/ai-retros/entries/` (or its non-repo equivalent), dedup within each scope before comparing across them, and attribute every theme to its scope(s) in the brief.

## The brief

Exactly this structure:

```markdown
# AI Collaboration Retro Brief — <date range>, <N> sessions
**Scope:** <the review scope: repo / product / account / engagement — list the sources if rolled up>
**Skill:** ai-retro-brief v<version from this file's frontmatter>

## Recurring friction
<Themes ranked by frequency × cost. Each theme: prevalence (n of N sessions), representative dated examples, root-cause label, the fixes entries proposed and their altitudes. If a theme is arguably deliberate friction — a control point the team may want to keep (review gates, approval steps) — present it as a keep-or-kill question, not a fix.>

## Root-cause distribution
<Counts per root-cause label, then group them: briefing (ambiguous-instruction, missing-context, incorrect-context, changed-requirements) · documentation (missing-documentation, incorrect-documentation) · work material (work-material-friction — name the concrete material: codebase, account, board, template) · tooling (missing-access-or-tool, environment-friction) · agent (agent-error). State plainly which group dominates — that tells the team whether their attention goes to process, docs, the work material, tooling, or the agent setup. If entries carry altitude tags, also state where proposed fixes are landing.>

## Were suggested fixes adopted?
<For each past "Do this first": did the same friction stop appearing afterward? Unadopted repeat suggestions deserve the team's attention most.>

## Top 3 recommended actions
<Each tied to specific evidence from specific entries, and each naming its altitude. Concrete enough to become a retro action item — owner-assignable, checkable.>

## Trend
<Is friction per session going up, down, or flat? One paragraph, honest about small-sample uncertainty.>
```

## Rules

- Every claim traces to specific dated entries. No pattern claims from a single occurrence.
- Do not soften findings: if most friction traces to briefing quality, say that; if most traces to agent error, say that too.
- Root-cause counts use the fixed vocabulary as-is; if an entry contains an out-of-vocabulary label, flag it rather than silently reclassifying — flags accumulate into the vocabulary review.
- Exclude backfilled entries from the "were fixes adopted?" analysis (they weren't written in sequence), but include their friction in counts.
- Costs are agent-estimated and soft — carry `(est.)` markers through, and treat the ranking as attention allocation, not accounting.
- Write rolled-up briefs for the widest audience they will reach: prefer evidence pointers (scope, date, entry) over quotes when a theme crosses scope boundaries, and never carry one scope's sensitive detail into another scope's review.
- Keep it to one page. The brief feeds a conversation; it doesn't replace one.

## Vocabulary review (every ~5th brief, or quarterly)

If 5+ briefs have accumulated since the last vocabulary review — or a quarter has passed — append a **Vocabulary review** section to this brief: label counts across the period; labels never used; strained fits and out-of-vocabulary flags; split or merge proposals with evidence. Proposals go to the taxonomy's canonical doc (docs-first) — never change labels in the skills directly; accepted changes arrive as a version bump. Keep the vocabulary small: an addition must displace something or carry its weight.

Save as `docs/ai-retros/briefs/brief-YYYY-MM-DD.md` (dated, so successive briefs accumulate), commit it, and present it to the user.

## Why this shape (provenance)

- **Clustering labeled findings into ranked patterns** is *error analysis* into a *failure taxonomy* ([Husain & Shankar, evals FAQ](https://hamel.dev/blog/posts/evals-faq/); the reference for fixed-vocabulary reliability is MAST, κ = 0.88 — [arXiv 2503.13657](https://arxiv.org/abs/2503.13657)). The brief is the team-scale half of Rahul Garg's *Feedback Flywheel* ([martinfowler.com](https://martinfowler.com/articles/reduce-friction-ai/feedback-flywheel.html)).
- **Keep-or-kill exists because not all friction is waste** — some friction is deliberate control ("Friction is what's necessary… to steer" — [Ronacher, AIE Europe](https://tldrecap.tech/posts/2026/aie-europe/ai-agents-friction/); Thoughtworks Radar v34 warns of *cognitive debt* from over-frictionless agent work — [announcement](https://www.thoughtworks.com/about-us/news/2026/combat-ai-cognitive-debt-radar-v34)). Eliminating a control point is a team decision, not a default.
- **Costs stay soft** because model self-estimates are unreliable in exactly this register — LLM judgment flips ~13.6% on identical inputs ([arXiv 2606.13685](https://arxiv.org/abs/2606.13685)); the ranking allocates attention, it isn't accounting.
- **The vocabulary review** keeps the emergent school's virtue inside a fixed vocabulary: categories still emerge from the data (your evaluation strategy *"should emerge from observed failure patterns"* — [Husain & Shankar](https://hamel.dev/blog/posts/evals-faq/)), but at a dated, versioned review instead of mid-entry improvisation.
