<!-- markdownlint-disable -->

# Hardening Report: huggingface--semver-release-action/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **huggingface--semver-release-action/v1.1.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow uses action references pinned to mutable branch or tag refs instead of full 40-character commit SHAs. In latest-release.yaml: `actions/checkout@master` (branch ref) and `EndBug/latest-tag@v1` (tag ref). These can be silently changed by the upstream repository, enabling supply-chain attacks.

Locations:

- `.github/workflows/latest-release.yaml:11`
- `.github/workflows/latest-release.yaml:14`

### unpinned-uses (severity: high)

Workflow uses action references pinned to mutable tag refs instead of full 40-character commit SHAs. In lint.yml: `actions/checkout@v3` (tag ref) and `actions/setup-node@v3` (tag ref). These can be silently changed by the upstream repository, enabling supply-chain attacks.

Locations:

- `.github/workflows/lint.yml:14`
- `.github/workflows/lint.yml:17`

### missing-permissions (severity: medium)

Workflow file latest-release.yaml has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially write) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/latest-release.yaml:1`

### missing-permissions (severity: medium)

Workflow file lint.yml has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially write) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/lint.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings across two workflow files:

**latest-release.yaml:**
- Pinned `actions/checkout@master` → `@61b9e3751b92087fd0b06925ba6dd6314e06f089 # master`
- Pinned `EndBug/latest-tag@v1` → `@8fcae8848c1e23fd8212258f69a9619bc62cad67 # v1`
- Added top-level `permissions: {}` and job-level `permissions: contents: write` (required for the latest-tag action to push tags)

**lint.yml:**
- Pinned `actions/checkout@v3` → `@f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3`
- Pinned `actions/setup-node@v3` → `@3235b876344d2a9aa001b8d1453c930bba69e610 # v3`
- Added top-level `permissions: {}` and job-level `permissions: contents: read` (minimum needed for checkout)

