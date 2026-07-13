Run a retrospective on this working session and produce a log entry.

Review the actual conversation and collect concrete moments: instructions that were ambiguous and forced a guess, context or access that was missing, code or docs that made the work slower than they should have, work redone after a correction, tool failures, and things that made the work fast. Every observation must cite one of these moments — if you can't cite a moment, drop it.

Honesty rules:
- No praise, no filler. If a section has nothing real, write "Nothing notable".
- Critique my inputs (unclear brief, missing context, late requirements) — describe the effect, not a judgment. Critique your own errors too, and say when you're uncertain you caught them all.
- Never quote secrets, credentials, tokens, or customer data — this entry gets committed.
- Label every friction item with exactly one root cause from this fixed list (never invent labels; pick the closest): ambiguous-instruction, missing-context, incorrect-context, missing-documentation, incorrect-documentation, codebase-friction, missing-access-or-tool, agent-error, changed-requirements, environment-friction.
- Every friction item ends with a concrete, ticket-sized proposed fix. No finding without a fix.

Output exactly this format:

```markdown
# YYYY-MM-DD — <one-line task summary>

**Session size:** <e.g. "~40 turns, 3 deliverables">
**Outcome:** <shipped / partially shipped / blocked>
**Skill:** prompt-pack v1.2

## Went well
- <max 3 items, each with evidence>

## Friction
- **[root-cause]** <what happened, when, what it cost>
  → Fix: <concrete next step>

## Guesses made
- <decision + assumption; flag any still-unverified>

## Do this first
<the single highest-leverage fix, with its payoff in one line>
```

If `docs/ai-retros/entries/` already has entries, read the most recent one and add one line noting whether its "Do this first" was adopted this session or the same friction recurred.

Show me the entry for correction before it's final. I'll save it as `docs/ai-retros/entries/YYYY-MM-DD-<slug>.md` (or save and commit it yourself if you can write files).
