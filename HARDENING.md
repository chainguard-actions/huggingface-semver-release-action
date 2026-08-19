<!-- markdownlint-disable -->

# Hardening Report: huggingface--semver-release-action/v1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **huggingface--semver-release-action/v1.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions using mutable tags or branch names instead of pinned full-length commit SHAs, making the action vulnerable to supply-chain attacks.

`.github/workflows/latest-release.yaml`:
- `uses: actions/checkout@master` (branch ref — mutable)
- `uses: EndBug/latest-tag@v1` (tag ref — mutable)

`.github/workflows/lint.yml`:
- `uses: actions/checkout@v3` (tag ref — mutable)
- `uses: actions/setup-node@v3` (tag ref — mutable)

All of these should be replaced with the full 40-character commit SHA of the intended version.

Locations:

- `.github/workflows/latest-release.yaml:10`
- `.github/workflows/latest-release.yaml:13`
- `.github/workflows/lint.yml:11`
- `.github/workflows/lint.yml:14`

### missing-permissions (severity: medium)

Neither `.github/workflows/latest-release.yaml` nor `.github/workflows/lint.yml` declares a top-level `permissions:` key, and neither of their jobs declares a job-level `permissions:` key. Without explicit permissions, workflows run with the repository's default token permissions, which may be overly broad (e.g. write access to contents, packages, etc.). Each workflow should declare the minimal set of permissions required.

Locations:

- `.github/workflows/latest-release.yaml:1`
- `.github/workflows/lint.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

.github/workflows/latest-release.yaml:
- Pinned actions/checkout@master → @61b9e3751b92087fd0b06925ba6dd6314e06f089 # master
- Pinned EndBug/latest-tag@v1 → @8fcae8848c1e23fd8212258f69a9619bc62cad67 # v1
- Added top-level `permissions: {}` and job-level `permissions: contents: write` (required to create/update tags)

.github/workflows/lint.yml:
- Pinned actions/checkout@v3 → @f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3
- Pinned actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610 # v3
- Added top-level `permissions: {}` and job-level `permissions: contents: read` (needed for checkout)

