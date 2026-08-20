<!-- markdownlint-disable -->

# Hardening Report: ghalactic--github-release-from-tag/v6.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **ghalactic--github-release-from-tag/v6.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions and reusable workflows using mutable tags or branch names instead of immutable full-length SHA digests. This exposes the workflow to supply-chain attacks where a tag or branch could be silently updated to point to malicious code.

Failing references:
- ghalactic/repos/.github/workflows/shared-ci-action.yml@main (branch)
- ghalactic/repos/.github/workflows/shared-publish-release.yml@main (branch)
- actions/checkout@v5 (tag)
- JamesIves/github-pages-deploy-action@v4 (tag)

Locations:

- `.github/workflows/ci-action-scheduled.yml:10`
- `.github/workflows/ci-action.yml:10`
- `.github/workflows/publish-release-manual.yml:13`
- `.github/workflows/publish-release.yml:10`
- `.github/workflows/publish-schemas.yml:19`
- `.github/workflows/publish-schemas.yml:22`

### missing-permissions (severity: medium)

ci-action.yml and ci-action-scheduled.yml have no top-level `permissions:` key and their single job also has no `permissions:` key. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. Every job should declare minimal required permissions explicitly.

Locations:

- `.github/workflows/ci-action.yml:1`
- `.github/workflows/ci-action-scheduled.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Fixed missing-permissions by adding `permissions: {}` to ci-action.yml and ci-action-scheduled.yml. Fixed unpinned-uses for actions/checkout@v5 → @fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09 and JamesIves/github-pages-deploy-action@v4 → @fa24774553152dd7873cd16ebd8d959b010c5445 in publish-schemas.yml. NOTE: The ghalactic/repos/.github/workflows/shared-ci-action.yml@main and ghalactic/repos/.github/workflows/shared-publish-release.yml@main references in ci-action.yml, ci-action-scheduled.yml, publish-release.yml, and publish-release-manual.yml could not be pinned because the GitHub API returned 504 errors for the ghalactic/repos repository, making SHA resolution impossible.

### Iteration 2

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all four reusable workflow references from @main to the full commit SHA @043a734b4fc560617a6f9448e1532ae02b397321 # main in: ci-action.yml, ci-action-scheduled.yml, publish-release.yml, and publish-release-manual.yml. Both shared-ci-action.yml and shared-publish-release.yml references point to the same ghalactic/repos repository, so they share the same SHA.

