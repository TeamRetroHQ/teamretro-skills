# 2026-07-14 — Add rate-limit alerts to the public API

**Session size:** ~20 turns, 1 alert rule + runbook stub
**Outcome:** shipped
**Skill:** ai-session-retro v2.2

## Went well
- The existing alert rules in `monitoring/alerts/` gave a working pattern to copy; the new rule passed lint first time (turn 6).

## Friction
- **[missing-documentation]** No record of the per-plan rate limits; read three config files to reconstruct them before setting thresholds (turns 3–9, ~15 min (est.)).
  → Fix (altitude: docs): add `docs/api/rate-limits.md` listing each plan's limit and where it is configured.

## Guesses made
- Assumed alerts should fire at 80% of the limit. Unverified; confirm with the on-call owner.

## Do this first
Add `docs/api/rate-limits.md` listing each plan's rate limit and its config location. ~15 minutes — stops future sessions reverse-engineering limits from config.

Filed: parked-item xY7zAbCdEfGhIjKlMnOpQ (2026-07-14)
