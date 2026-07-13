# teamretro-skills

Skills for teams whose AI agents do real work — and should have a voice in how the team improves.

Your AI teammates work inside your repos every day. They hit the missing docs, the stale guides, the tech debt, the unclear briefs — and when the session ends, all of that evaporates. These skills close the loop: the agent gathers its own reflections at the end of a session, and brings them to your team's retrospective.

**None of these skills require TeamRetro.** The practice works with a markdown log in any repo and any retro process. If your team does use [TeamRetro](https://www.teamretro.com), the agent can additionally file its findings straight into your next retro via the TeamRetro MCP server.

## The skills

| Skill | What it does | Needs TeamRetro? |
|---|---|---|
| [`ai-session-retro`](skills/ai-session-retro/SKILL.md) | At the end of a working session, the agent logs an honest, evidence-cited retro entry: what went well, what caused friction (with a root-cause label and a concrete fix), what it guessed at, and the one fix worth doing first. | No — optional MCP filing |
| [`ai-retro-brief`](skills/ai-retro-brief/SKILL.md) | Reads the accumulated entries and produces a one-page brief for your team retrospective: recurring friction ranked by frequency × cost, whether past fixes stuck, top 3 recommended actions. | No |

Planned: skills that work directly with the TeamRetro MCP server (filing parked items and actions, prepping retros from team activity). Watch this repo.

## Install

**One command, any agent** (Claude Code, Cursor, Codex, Copilot, Amp, Cline, and more) — from your repo root:

```bash
npx skills add TeamRetroHQ/teamretro-skills
```

**Claude Code plugin** — installs the skill set and keeps it updated:

```
/plugin marketplace add TeamRetroHQ/teamretro-skills
/plugin install teamretro-skills@teamretro
```

**Manual copy** — a skill is just a folder; copy it into your repo's `.claude/skills/` (or `~/.claude/skills/` for all projects):

```bash
mkdir -p .claude/skills
cp -r skills/ai-session-retro skills/ai-retro-brief .claude/skills/
```

**No skill support at all?** Use the [prompt pack](prompt-pack/) — the same practice as a plain prompt + template.

## How it works

1. At the end of a substantial AI session, say **"run a retro on this session."** (~1 minute; you sanity-check the entry before it's final.)
2. Entries accumulate in `docs/ai-retros/entries/`, one markdown file each, committed to the repo. Every friction item carries one of ten fixed root-cause labels and a ticket-sized fix.
3. Before your team retro, ask for a brief: **"synthesize our AI retros."** The agent brings one page of ranked, evidence-backed patterns to the meeting.

Two rules keep the log useful: **no finding without a proposed fix**, and **the honesty runs both ways** — the agent reports what the repo and the briefs made hard, and its own mistakes.

See [examples/](examples/) for a sample entry and brief.

## Status

v1.2 — pre-release. Eval definitions for both skills are in `skills/*/evals/`; the v1.2 eval run is in progress. Pilot feedback welcome via issues.

## License

[MIT](LICENSE)
