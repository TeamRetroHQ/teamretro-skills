# 2026-07-13 — Add reactions to standup updates

*(Illustrative example — not a real session.)*

**Session size:** ~35 turns, 1 feature + tests
**Outcome:** shipped
**Skill:** ai-session-retro v1.2

## Went well
- Found and reused the existing emoji-picker component instead of building a new one — the component index in CLAUDE.md paid off (turn 6).

## Friction
- **[missing-documentation]** Nothing says which of the two notification pipelines is current; wired into the deprecated one first and redid the integration after review (turns 12–19).
  → Fix: one line in CLAUDE.md naming the current pipeline. ~5 minutes.
- **[codebase-friction]** Three different modal patterns in the codebase; matched the oldest one and the reviewer asked for the newest — cost a full revision round (turns 21–27).
  → Fix: declare one standard modal pattern; ticket the migration of the other two.
- **[agent-error]** Missed the existing feature-flag registry and hard-coded the rollout; caught in review (turn 29).
  → Fix: list the feature-flag registry location in CLAUDE.md so no session can miss it.

## Guesses made
- Assumed reactions should not notify the whole team — no spec either way. Still unverified; needs a product call.

## Do this first
Add five lines to CLAUDE.md: current notification pipeline, standard modal pattern, feature-flag registry location. ~15 minutes, one owner — removes three of this session's four detours for every future session, human or AI.

## Also worth doing
- Confirm the reactions-notification question in #product before launch.

Loop check: previous entry (2026-07-10) suggested adding the test-data seeding script to the README — adopted; setup friction did not recur this session.
