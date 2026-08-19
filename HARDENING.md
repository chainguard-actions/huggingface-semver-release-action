<!-- markdownlint-disable -->

# Hardening Report: huggingface--semver-release-action/v1.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **huggingface--semver-release-action/v1.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable tags or branch names instead of pinned 40-character commit SHAs, making them vulnerable to supply-chain attacks if the referenced tag or branch is moved or compromised.

.github/workflows/latest-release.yaml:
  - uses: actions/checkout@master  (line 12) — branch ref, not a SHA
  - uses: EndBug/latest-tag@v1     (line 15) — tag ref, not a SHA

.github/workflows/lint.yml:
  - uses: actions/checkout@v3      (line 16) — tag ref, not a SHA
  - uses: actions/setup-node@v3    (line 19) — tag ref, not a SHA

Locations:

- `.github/workflows/latest-release.yaml:12`
- `.github/workflows/latest-release.yaml:15`
- `.github/workflows/lint.yml:16`
- `.github/workflows/lint.yml:19`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no individual job within either file declares its own `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

.github/workflows/latest-release.yaml — no top-level or job-level permissions declared.
.github/workflows/lint.yml — no top-level or job-level permissions declared.

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
- Added top-level permissions: contents: write (required for EndBug/latest-tag to push the tag)

.github/workflows/lint.yml:
- Pinned actions/checkout@v3 → @f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3
- Pinned actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610 # v3
- Added top-level permissions: contents: read (minimum needed for checkout)

