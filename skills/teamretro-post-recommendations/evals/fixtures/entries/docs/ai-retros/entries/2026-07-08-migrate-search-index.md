# 2026-07-08 — Migrate search index to new analyzer

**Session size:** ~25 turns, 1 migration
**Outcome:** shipped
**Skill:** ai-session-retro v1.2

## Went well
- The reindex dry-run flag let me validate the mapping before touching prod data (turn 11).

## Friction
- **[ambiguous-instruction]** "Improve search" didn't say whether the goal was recall or latency; tuned for recall and the reviewer wanted latency, so the analyzer config was redone (turns 5–13).
  → Fix: capture the search objective (recall vs latency vs relevance) in the ticket template. ~10 minutes.

## Guesses made
- Assumed synonyms should be applied at query time, not index time. Verified with the team at turn 18.

## Do this first
Add a "search objective" field to the ticket template so the target metric is stated before work starts. ~10 minutes — removes the guess that cost a rework this session.

Posted: parked-item aB3dEfGh1jKlMnOpQrStU (2026-07-09)
