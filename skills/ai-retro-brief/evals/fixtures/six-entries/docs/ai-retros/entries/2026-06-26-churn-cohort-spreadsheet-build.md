# 2026-06-26 — Churn cohort spreadsheet build

**Session size:** ~22 turns, 1 xlsx
**Outcome:** partial
**Skill:** ai-session-retro v1.2

## Went well
- Nothing notable

## Friction
- **[missing-access-or-tool]** No access to the analytics database; user pasted CSV extracts in 5 separate messages, two had mismatched columns (turns 3-12) — cost: manual reconciliation, session ended before charts done.
  → Fix: Connect a read-only analytics data source so extracts don't have to be hand-pasted.
- **[ambiguous-instruction]** 'Cohort' meant signup-month cohorts to the user but plan-tier cohorts were built first (turn 8) — cost: sheet rebuilt.
  → Fix: Name the cohort dimension (signup month vs plan tier) in the first message of any cohort request.

## Guesses made
- Excluded free-tier users from churn calc — assumed: Paid churn is the metric that matters (still unverified).

## Do this first
Connect a read-only analytics data source so extracts don't have to be hand-pasted.
