# Contributing

Thanks for your interest in improving these skills. Issues and pull requests are welcome.

## Ways to contribute

- **Report friction:** if a skill produced a bad entry, a vague fix, or missed something obvious, tell us at [feedback.teamretro.com](https://feedback.teamretro.com) (or open a GitHub issue) with the (redacted) output and what you expected. This is the most valuable feedback we can get.
- **Improve a skill:** PRs to `skills/*/SKILL.md` are welcome. Keep the core rules intact: evidence-cited findings only, no finding without a proposed fix, the ten root-cause labels stay unbranded and fixed.
- **Add prompt-pack variants:** adaptations for other agents belong in `prompt-pack/`.

## Ground rules for skill changes

- **Don't change the label taxonomy** (the ten root-cause labels) without an issue discussion first — briefs and tooling depend on it being stable.
- **Update the evals** alongside any behavior change: each skill has an eval suite in `skills/<skill>/evals/`. A skill edit that changes expected output should change the eval expectations in the same PR.
- **Fixtures are illustrative** — never include real customer data, secrets, or content from private repos in fixtures or examples.
- **Bump the version** in the skill's frontmatter (`metadata.version`) and note the change in `CHANGELOG.md`.

## Process

1. Fork (or branch, for maintainers) off `main`.
2. Make the change, including eval and CHANGELOG updates.
3. Open a PR — small and focused beats broad. Maintainers review and merge; `main` only takes changes via PR.

## Security

Please don't report security issues in public issues — see [SECURITY.md](SECURITY.md).

## Conduct

Be respectful and constructive. We follow the spirit of the [Contributor Covenant](https://www.contributor-covenant.org/); reports of unacceptable behavior can go to [security@teamretro.com](mailto:security@teamretro.com).
