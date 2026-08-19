<!-- markdownlint-disable -->

# Hardening Report: Gr1N--setup-poetry/v8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Gr1N--setup-poetry/v8** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable tags rather than immutable 40-character commit SHAs, making the workflows vulnerable to supply-chain attacks if the referenced tags are moved or overwritten.

In `.github/workflows/default.yml`:
- `uses: actions/checkout@v3` (tag, not SHA)
- `uses: actions/cache@v3` (tag, not SHA)
- `uses: actions/setup-python@v4` (tag, not SHA)

In `.github/workflows/watch-started.yml`:
- `uses: appleboy/telegram-action@0.0.7` (version tag, not SHA)

All of these should be pinned to their full 40-character commit SHA, e.g. `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/default.yml:13`
- `.github/workflows/default.yml:14`
- `.github/workflows/default.yml:33`
- `.github/workflows/default.yml:34`
- `.github/workflows/watch-started.yml:9`

### missing-permissions (severity: medium)

Neither `.github/workflows/default.yml` nor `.github/workflows/watch-started.yml` declares a top-level `permissions:` block, and no individual job within either file declares job-level permissions. Without explicit permissions, workflows run with the default (often broad) token permissions, which violates the principle of least privilege. Each workflow should declare `permissions: {}` at the top level and grant only the specific scopes required.

Locations:

- `.github/workflows/default.yml:1`
- `.github/workflows/watch-started.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all 4 unique action references to their full 40-character commit SHAs with tag comments for readability — actions/checkout@v3, actions/cache@v3, actions/setup-python@v4, and appleboy/telegram-action@0.0.7. (2) Added `permissions: {}` top-level block to both .github/workflows/default.yml and .github/workflows/watch-started.yml to enforce least-privilege token access.

