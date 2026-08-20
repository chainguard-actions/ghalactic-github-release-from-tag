<!-- markdownlint-disable -->

# Hardening Report: ghalactic--github-release-from-tag/v6.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **ghalactic--github-release-from-tag/v6.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions and reusable workflows using mutable tags or branch names instead of full 40-character commit SHA digests, making them vulnerable to supply-chain attacks if the referenced ref is moved or compromised.

Failing references:
- ci-action-scheduled.yml: `uses: ghalactic/repos/.github/workflows/shared-ci-action.yml@main`
- ci-action.yml: `uses: ghalactic/repos/.github/workflows/shared-ci-action.yml@main`
- publish-release-manual.yml: `uses: ghalactic/repos/.github/workflows/shared-publish-release.yml@main`
- publish-release.yml: `uses: ghalactic/repos/.github/workflows/shared-publish-release.yml@main`
- publish-schemas.yml: `uses: actions/checkout@v5`
- publish-schemas.yml: `uses: JamesIves/github-pages-deploy-action@v4`

Locations:

- `.github/workflows/ci-action-scheduled.yml:8`
- `.github/workflows/ci-action.yml:6`
- `.github/workflows/publish-release-manual.yml:10`
- `.github/workflows/publish-release.yml:10`
- `.github/workflows/publish-schemas.yml:14`
- `.github/workflows/publish-schemas.yml:17`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, violating the principle of least privilege.

- ci-action-scheduled.yml: single job `ci` has no permissions block
- ci-action.yml: single job `ci` has no permissions block

Locations:

- `.github/workflows/ci-action-scheduled.yml:1`
- `.github/workflows/ci-action.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 unpinned action references by resolving them to full commit SHAs: ghalactic/repos@main → 043a734b4fc560617a6f9448e1532ae02b397321, actions/checkout@v5 → fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09, JamesIves/github-pages-deploy-action@v4 → fa24774553152dd7873cd16ebd8d959b010c5445. Added `permissions: {}` to the `ci` job in ci-action-scheduled.yml and ci-action.yml to enforce least privilege. The publish-release.yml, publish-release-manual.yml, and publish-schemas.yml workflows already had explicit permissions blocks.

