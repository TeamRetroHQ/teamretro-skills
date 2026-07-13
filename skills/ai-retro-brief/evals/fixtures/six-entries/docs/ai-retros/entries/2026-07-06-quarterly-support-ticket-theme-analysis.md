# 2026-07-06 — Quarterly support-ticket theme analysis

**Session size:** ~25 turns, 1 deliverable
**Outcome:** shipped
**Skill:** ai-session-retro v1.2

## Went well
- Help Scout connector meant ticket data was pulled directly (turns 2-4)

## Friction
- **[ambiguous-instruction]** 'Recent tickets' wasn't defined; agent assumed 30 days, user wanted the full quarter — first analysis discarded (turn 9) — cost: first analysis discarded.
  → Fix: State the time window explicitly in the first message when asking for any 'recent' data analysis.
- **[missing-context]** Internal tag taxonomy wasn't explained; billing and refund themes mixed (turn 12) — cost: regrouping pass.
  → Fix: Document the support tag taxonomy (one page) so grouping doesn't mix billing and refund themes.

## Guesses made
- Treated 'themes' as qualitative clusters — assumed: Narrative insight wanted (verified).

## Do this first
State the time window explicitly in the first message when asking for any 'recent' data analysis.
