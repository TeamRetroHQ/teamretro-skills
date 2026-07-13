# 2026-07-06 — Quarterly support-ticket theme analysis

**Session size:** ~25 turns, 1 deliverable
**Outcome:** shipped
**Skill:** session-retro v1.1

## Went well
- Help Scout connector meant ticket data was pulled directly, no manual export (turns 2-4).

## Friction
- **[ambiguous-instruction]** "Recent tickets" wasn't defined; agent assumed 30 days, user wanted the full quarter — first analysis discarded (turn 9).
- **[missing-context]** The internal tag taxonomy wasn't explained; agent grouped by raw tags which mixed billing and refund themes (turn 12).

## Guesses made
- Assumed "themes" meant qualitative clusters, not tag counts. Verified correct at turn 14.

## One change
State the time window explicitly in the first message when asking for any "recent" data analysis.
