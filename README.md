# engineering-playbook
Engineering standards and workflows for designing, building, maintaining, testing, and shipping software consistently.

## Skills

This repository includes a reusable, on-demand changelog skill under [`.agents/skills/`](.agents/skills/).

### Changelog and release notes

The [changelog-skill](.agents/skills/changelog-skill/SKILL.md) creates or updates `CHANGELOG.md` from project changes, and requires a note for every PR. User-visible changes go under the appropriate release section; non-user-facing work goes under `Internal`. PR work does not bump the product version. Release preparation synchronizes the versioned changelog heading with the project's version source file, commonly `package.json`.

For reliable use in every PR, install the skill in a directory supported by your agent and add the [project-instructions snippet](.agents/skills/changelog-skill/references/PROJECT-INSTRUCTIONS.md) to its always-loaded project instructions. The skill is portable across agent hosts that support the `SKILL.md` format; automatic discovery and instruction filenames vary by host.
