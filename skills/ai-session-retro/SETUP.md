# Setup — making capture deterministic

The skill's weakest point is that invocation is probabilistic: if nothing triggers the retro, the log starves silently and the first brief never has enough to say. The fix is to layer a **deterministic trigger** (hook or command) with the **skill as payload** and a **convention line** as the norm. Pick at least one trigger below; two is better.

## 1. Session-start reminder (hook — fires every session)

Claude Code: add to `.claude/settings.json` (project) or `~/.claude/settings.json` (personal):

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Team practice: at the natural end of a substantial session, run the ai-session-retro skill and commit the entry.'"
          }
        ]
      }
    ]
  }
}
```

The hook's output is injected as session context — the agent starts every session already knowing the practice exists. Blunt (it fires on two-minute sessions too), but it's the only trigger that depends on nobody remembering anything.

## 2. End-of-session command (deliberate trigger)

Claude Code: create `.claude/commands/retro.md`:

```markdown
Run the ai-session-retro skill on this session. Follow it exactly: review the actual
session, label each friction item from the fixed vocabulary, propose a ticket-sized
fix with an altitude tag, commit the entry, then show it to me.
```

Typing `/retro` at the end of a feature branch or a triage shift invokes the skill on demand. Deterministic once typed; the weak link is human memory — pair it with trigger 1 or 3.

## 3. Convention line (travels with the scope)

Add one line to the scope's context file (`CLAUDE.md` / `AGENTS.md`) or, for non-code scopes, to the team SOP / workspace instructions:

```
End every substantial AI-assisted session by running ai-session-retro and committing the entry.
```

Zero infrastructure. It decays as the file grows, so treat it as the norm layer, not the trigger.

## 4. Your first entry (what good looks like)

```markdown
# 2026-07-16 — Monthly ads account review

**Session size:** ~25 turns, 1 deliverable
**Outcome:** shipped
**Skill:** ai-session-retro v2.1

## Went well
- Exec summary shipped in one pass; the account structure doc from last month held up

## Friction
- **[missing-context]** The brief didn't say the French campaigns were deliberately
  paused for Q3; ~40 min (est.) auditing a "broken" campaign that was fine.
  → Fix (altitude: process): add campaign-status flags to the monthly brief template

## Guesses made
- Assumed the target CPA unchanged from June — unverified

## Do this first
Campaign-status flags in the brief template — kills the largest single time sink this month.
```

Notes: exactly one label per finding; evidence with a cost (`(est.)` when soft); a ticket-sized fix with an altitude; nothing that identifies a person; no secrets, keys, or customer data (see the skill's never-log list).

## 5. Where the log lives

At the work's **review scope** — the boundary where fixes land. Repo for most dev work (`docs/ai-retros/entries/`). For non-code scopes (an ads account, a support inbox, a client engagement): the same folder structure in that workspace's document home. One log per scope; identical schema everywhere — the schema, not the storage, is what makes entries aggregate later.

After ~5 entries, run the companion `ai-retro-brief` skill before your team's next retrospective.
