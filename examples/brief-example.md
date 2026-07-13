# AI Collaboration Retro Brief — 2026-06-22 → 2026-07-13, 8 sessions

*(Illustrative example — not real data.)*

Stamped: ai-retro-brief v1.2

## Recurring friction
1. **Undocumented "which one is current?" decisions** — 5 of 8 sessions (`missing-documentation`). Two notification pipelines, two auth helpers, three modal patterns; agents (and one new hire) picked the deprecated option first in every case. Proposed fixes all point the same way: short "current vs deprecated" section in CLAUDE.md.
2. **Ambiguous briefs on deliverables** — 4 of 8 sessions (`ambiguous-instruction`). Audience, time window, or format arrived mid-session; two full redos. Entries repeatedly propose a three-line brief (audience, length/format, scope) before work starts.
3. **Modal/tech-debt inconsistency** — 3 of 8 sessions (`codebase-friction`). Each occurrence cost a review round.

## Root-cause distribution
missing-documentation 5 · ambiguous-instruction 4 · codebase-friction 3 · agent-error 2 · missing-access-or-tool 2 · changed-requirements 1 · environment-friction 1
Groups: **documentation 5** · briefing 5 · codebase 3 · tooling 3 · agent 2.
Documentation and briefing dominate jointly — the highest-leverage fixes are docs and process, not the model.

## Were suggested fixes adopted?
- "State the time window explicitly" (2026-07-06) — **adopted**; no recurrence in 3 subsequent sessions.
- "Document the current notification pipeline" (2026-07-01) — **not adopted**; recurred twice since. This is the one to take into the retro.
- "Verify pricing claims with live search" (2026-06-30) — not verifiable from the log (no comparable session since).

## Top 3 recommended actions
1. Add a "current vs deprecated" section to CLAUDE.md covering notification pipeline, auth helper, modal pattern, feature-flag registry (entries 07-01, 07-08, 07-13). ~30 min, one owner.
2. Adopt the three-line brief for deliverable requests: audience, length/format, scope (entries 06-22, 07-02, 07-06).
3. Declare the standard modal pattern and ticket migration of the legacy two (entries 07-08, 07-13).

## Trend
Friction per session is flat (~2.1 items/session) but its mix is shifting from briefing toward documentation as briefs improve — consistent with the time-window fix being adopted. Eight sessions is a small sample; treat direction, not magnitude, as the signal.
