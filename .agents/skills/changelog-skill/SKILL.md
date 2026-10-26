---
name: changelog-skill
description: Create and maintain a project's CHANGELOG.md automatically from code changes. Use when implementing work, creating or updating a pull request, reviewing a pull request for changelog coverage, initializing a changelog, or preparing a release and synchronizing its version with package.json.
license: MIT
metadata:
  author: copilot
  version: "1.0.0"
---

# Changelog Skill

Maintain a changelog as part of the implementation and pull request workflow. Create `CHANGELOG.md` if the project does not have one; otherwise preserve its existing format where practical and update its top `Unreleased` section. Record every pull request, including internal-only work, without bumping the product version for each PR.

## When to run

- When implementing or changing code, tests, docs, build configuration, or tooling.
- Before creating or updating a pull request, compare the complete PR diff with `CHANGELOG.md` and reconcile its entry.
- When asked to create, audit, or prepare a release.

For reliable automatic use on every PR, the user should also install the companion [project instructions](./references/PROJECT-INSTRUCTIONS.md) in the project's always-loaded agent instructions.

## PR workflow

1. Inspect the repository instructions, existing `CHANGELOG.md`, the PR's complete diff against its base branch, and any existing changelog changes on the branch.
2. If `CHANGELOG.md` is missing, create it using the format below. Do not assume the project has `package.json`; identify the actual version source file if the project has one.
3. If `CHANGELOG.md` already exists but has no `Unreleased` heading, add one at the top without changing or deleting its existing release history.
4. Add or revise an entry under the top `## [Unreleased]` heading. Do not create a versioned release heading or change the version source file for ordinary PR work.
5. Categorize each PR:
   - Use `Added`, `Changed`, `Fixed`, `Removed`, `Deprecated`, or `Security` for user-visible changes.
   - Use `Internal` for changes that are not user-visible, such as tests, refactoring, documentation for contributors, build tooling, or dependency maintenance.
6. Describe the result accurately from the diff, not just from commit messages or the PR title. Keep entries concise, avoid duplicating existing entries, and preserve unrelated notes already in `Unreleased`.
7. Before creating or updating a PR, confirm that it contains an appropriate entry. If a PR mixes user-facing and internal work, record both under the relevant sections. Do not claim an entry was added if the working tree or PR diff does not contain it.

An entry is expected for every PR. Use `Internal` rather than omitting a note when work has no user-facing impact. If the intent or impact cannot be determined from the code and PR context, ask the user instead of inventing claims.

When working with multiple branches or concurrent PRs, re-read the latest changelog before editing and preserve changes already merged or added by others. Resolve conflicts by combining non-duplicate entries, not by replacing the whole file.

## Changelog format

Create the following header for a new changelog, adapting the product placeholder if known:

```markdown
# Changelog

All notable changes to <Product> are documented here.

<!--
Add an entry under Unreleased for every pull request.
Use user-facing sections for product changes and Internal for work customers do not see.
Do not bump the product version for pull requests.
At release time, replace Unreleased with a dated semver heading and start a fresh Unreleased section.
-->

## [Unreleased]
```

Use these section names as needed:

- `Added`
- `Changed`
- `Fixed`
- `Removed`
- `Deprecated`
- `Security`
- `Internal`

Only create a section when it has an entry. Each entry should be a bullet with a short, specific description:

```markdown
### Fixed

- **Exported reports** — fixed totals that did not reflect active filters.

### Internal

- **Test coverage** — added regression tests for report filtering.
```

Use bold titles when they improve scanability; do not invent user-facing benefits for internal changes. Indented bullets may supply additional context. Keep prior entries intact and add new information without duplicates.

## Release workflow

Do not choose or bump a version during ordinary PR work. When explicitly preparing a release:

1. Read the current version from the project's version source file. In Node.js projects, this is commonly the `version` field in `package.json`.
2. Determine the next semver version from the scope of the release and the project's release policy. Do not guess when the policy or breaking-change status is unclear; ask.
3. Replace the top `## [Unreleased]` heading with `## [x.y.z] — YYYY-MM-DD`, using the chosen version and release date.
4. Keep all entries, including `Internal`, in the versioned changelog to preserve the PR history. Exclude `Internal` from customer-facing Release Notes views.
5. Add a fresh `## [Unreleased]` section above the released version for subsequent PRs.
6. Update the version source file and any project-required package version fields in the same release change. Check that these values agree with the release heading.
7. Validate the resulting changelog and version changes using the project's existing scripts or tests.

Versioned headings must use semver `x.y.z` and a valid `YYYY-MM-DD` date. The newest release comes first. Do not edit older release notes except to correct an error.

## Release Notes display

If the application renders a runtime Release Notes view:

- Parse `CHANGELOG.md` as data rather than duplicating release notes in UI strings.
- Read the installed version from the project's version source file or existing runtime version API, not a copied constant.
- Exclude `Unreleased` and `Internal` content from customer-facing Release Notes.
- Render released notes only; compare the installed version with published versioned headings, not with `Unreleased`.
- Build inline Markdown using safe DOM nodes and `textContent`, never `innerHTML`. Allow only the link schemes and in-app routes the product explicitly supports.

Do not build a runtime UI unless the user asks for one or the project already has one that needs updating.

## Validation

Before finishing, verify:

- Every PR represented by the current change has an appropriate entry under `Unreleased`.
- Internal-only changes use `Internal`; customer-facing notes do not include internal entries.
- Existing `Unreleased` entries and prior releases remain intact.
- No version was bumped merely for creating or updating a PR.
- At release time, the release heading, date, and version source agree.
- The project's existing changelog/version validation passes, if available.

If the project has no validator, do not add scripts, hooks, or CI workflows unless asked. Clearly report any missing automation or unresolved release policy.
