# Project instruction for automatic changelog updates

Add the following rule to the project's always-loaded agent instructions (for example `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, or the equivalent project instructions for the agent host). Install the `changelog-skill` in a skill directory the host recognizes.

```markdown
## Changelog on every pull request

For every pull request, load and follow the `changelog-skill`. Before creating or updating the PR, compare the complete PR diff with `CHANGELOG.md` and add or revise an entry under its top `Unreleased` section. Record customer-facing changes in the appropriate section; use `Internal` for PRs without customer-visible impact. Preserve existing entries and do not bump the product version for ordinary PR work. If the changelog is missing or has no `Unreleased` heading, create or update it following the skill. If the PR has no changelog entry, do not consider the PR ready.
```

Skills are loaded on demand, and agent hosts differ in how they discover project skills. The instruction above makes the PR requirement explicit; install the skill and instruction using paths supported by the target agent.
