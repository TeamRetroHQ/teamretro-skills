Read every AI session retro entry in `docs/ai-retros/entries/` (I'll paste them if you can't read files) and synthesize a one-page brief for our team retrospective.

If there are fewer than 3 entries, say so and give only a light summary — pattern claims need repetition.

Rules:
- Every claim traces to specific dated entries. No pattern claims from a single occurrence.
- Don't soften findings: if most friction traces to our briefing quality, say that; if most traces to your own errors, say that too.
- Use the entries' root-cause labels as-is; flag out-of-vocabulary labels rather than reclassifying.
- One page. The brief feeds a conversation; it doesn't replace one.

Output exactly this structure:

```markdown
# AI Collaboration Retro Brief — <date range>, <N> sessions

## Recurring friction
<Themes ranked by frequency × cost: how many sessions, representative examples, root-cause label, proposed fixes.>

## Root-cause distribution
<Counts per label, grouped: briefing (ambiguous-instruction, missing-context, incorrect-context, changed-requirements) / documentation (missing-documentation, incorrect-documentation) / codebase (codebase-friction) / tooling (missing-access-or-tool, environment-friction) / agent (agent-error). State which group dominates.>

## Were suggested fixes adopted?
<For each past "Do this first": did that friction stop appearing? Unadopted repeat suggestions matter most.>

## Top 3 recommended actions
<Each tied to dated evidence; concrete enough to be an owner-assignable retro action.>

## Trend
<Friction per session: up, down, flat? One paragraph, honest about the small sample.>
```

Save it as `docs/ai-retros/briefs/brief-YYYY-MM-DD.md` if you can write files; otherwise I'll save it.
