# Changelog

## v1.2 (2026-07-13) — pre-release

Reworked from the v1.1 Cowork prototype for git-native, multi-repo use:

- Renamed: `session-retro` → `ai-session-retro`, `retro-brief` → `ai-retro-brief`.
- Per-entry files (`docs/ai-retros/entries/YYYY-MM-DD-slug.md`) replace the single append-only log — no merge conflicts, no interrupted-append corruption. Legacy `retro-log.md` still parsed by ai-retro-brief.
- New root-cause label: `codebase-friction` (tech debt, inconsistent patterns, confusing structure). Vocabulary is now 10 labels; brief adds a "codebase" group.
- No finding without a fix: every friction item ends with a ticket-sized "→ Fix:"; "One change" section replaced by "Do this first" (+ optional "Also worth doing", max 2).
- Retroactive mode: backfill entries from past-session transcripts, dated by session date, marked backfilled, excluded from adoption analysis.
- Commit guidance, no-secrets rule, git-repo-relative paths (Cowork "connected folder" wording removed).
- Trigger hygiene: session-scoped triggers only; explicit exclusion for retrospective-product work (creating/facilitating retros in TeamRetro or similar).
- Optional MCP filing step: offer once, after human confirmation, to file the "Do this first" fix as a parked item or action; record the filed ref; never auto-file, never create retrospectives/ideas.
- Evals split per skill, rewritten for v1.2, plus negative-trigger and backfill evals. v1.2 eval run pending.

## v1.1 (2026-07-13)

Cowork prototype: single append-only retro-log.md, 9-label vocabulary, benchmarked at 100% (3 evals x 3 runs) against the pre-1.1 jsonl-writing variant.
