<!-- markdownlint-disable -->

# Hardening Report: huggingface--semver-release-action/v1.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **huggingface--semver-release-action/v1.1.1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file '.github/workflows/latest-release.yaml' has no top-level 'permissions:' key and no job-level 'permissions:' key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions.

Locations:

- `.github/workflows/latest-release.yaml:1`

### missing-permissions (severity: medium)

The workflow file '.github/workflows/lint.yml' has no top-level 'permissions:' key and no job-level 'permissions:' key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions.

Locations:

- `.github/workflows/lint.yml:1`

### unpinned-uses (severity: high)

The workflow '.github/workflows/latest-release.yaml' references actions by mutable tag/branch refs instead of full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if those refs are moved: 'actions/checkout@master' (branch ref) and 'EndBug/latest-tag@v1' (tag ref).

Locations:

- `.github/workflows/latest-release.yaml:10`
- `.github/workflows/latest-release.yaml:13`

### unpinned-uses (severity: high)

The workflow '.github/workflows/lint.yml' references actions by mutable tag refs instead of full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks: 'actions/checkout@v3' and 'actions/setup-node@v3'.

Locations:

- `.github/workflows/lint.yml:13`
- `.github/workflows/lint.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Fixed both workflow files: (1) Added top-level 'permissions: {}' and minimal job-level permissions to both latest-release.yaml (contents: write for tag creation) and lint.yml (contents: read for checkout). (2) Pinned all four action references to full 40-character commit SHAs: actions/checkout@master→61b9e3751b92087fd0b06925ba6dd6314e06f089, EndBug/latest-tag@v1→8fcae8848c1e23fd8212258f69a9619bc62cad67, actions/checkout@v3→f43a0e5ff2bd294095638e18286ca9a3d1956744, actions/setup-node@v3→3235b876344d2a9aa001b8d1453c930bba69e610. Original tag refs preserved as inline comments.

