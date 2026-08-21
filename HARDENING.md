<!-- markdownlint-disable -->

# Hardening Report: ghalactic--github-release-from-tag/v5.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **ghalactic--github-release-from-tag/v5.4.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions and reusable workflows using mutable tags or branch names instead of pinned full-length SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced ref is updated maliciously.

Failing references:
- ci-action.yml: `ghalactic/repos/.github/workflows/shared-ci-action.yml@main`
- ci-action-scheduled.yml: `ghalactic/repos/.github/workflows/shared-ci-action.yml@main`
- publish-release.yml: `ghalactic/repos/.github/workflows/shared-publish-release.yml@main`
- publish-release-manual.yml: `ghalactic/repos/.github/workflows/shared-publish-release.yml@main`
- publish-schemas.yml: `actions/checkout@v4` and `JamesIves/github-pages-deploy-action@v4`

Locations:

- `.github/workflows/ci-action.yml:9`
- `.github/workflows/ci-action-scheduled.yml:10`
- `.github/workflows/publish-release.yml:9`
- `.github/workflows/publish-release-manual.yml:14`
- `.github/workflows/publish-schemas.yml:16`
- `.github/workflows/publish-schemas.yml:19`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` block and no job-level `permissions:` block on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, violating the principle of least privilege.

- ci-action.yml: no permissions defined at any level
- ci-action-scheduled.yml: no permissions defined at any level

Locations:

- `.github/workflows/ci-action.yml:1`
- `.github/workflows/ci-action-scheduled.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 unpinned action/workflow references by replacing mutable tags/branches with full commit SHAs (preserving original refs as comments). Added `permissions: {}` top-level blocks to ci-action.yml and ci-action-scheduled.yml to enforce least privilege. The publish-release.yml and publish-release-manual.yml already had job-level permissions blocks (contents: write, discussions: write), so only the unpinned uses were fixed there. publish-schemas.yml already had a job-level permissions block and only needed the action pins fixed.

