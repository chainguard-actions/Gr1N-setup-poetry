<!-- markdownlint-disable -->

# Hardening Report: Gr1N--setup-poetry/v9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Gr1N--setup-poetry/v9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use mutable tag/version refs instead of pinned 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.

In `.github/workflows/default.yml`:
- `uses: actions/checkout@v3` (line 13)
- `uses: actions/cache@v3` (line 14)
- `uses: actions/setup-python@v4` (line 44)

In `.github/workflows/watch-started.yml`:
- `uses: appleboy/telegram-action@0.0.7` (line 10)

All of these should be pinned to their full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`).

Locations:

- `.github/workflows/default.yml:13`
- `.github/workflows/default.yml:14`
- `.github/workflows/default.yml:44`
- `.github/workflows/watch-started.yml:10`

### missing-permissions (severity: medium)

Neither `.github/workflows/default.yml` nor `.github/workflows/watch-started.yml` declares a top-level `permissions:` key, and no individual job within either file declares its own `permissions:` block. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (which may be `write-all` for older repositories), granting jobs more access than they need. A minimal `permissions: {}` or specific scopes (e.g. `contents: read`) should be declared at the top level or per job.

Locations:

- `.github/workflows/default.yml:1`
- `.github/workflows/watch-started.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all 4 action references to full 40-char commit SHAs with original tags as comments. (2) Added top-level `permissions: {}` to both .github/workflows/default.yml and .github/workflows/watch-started.yml. Added per-job `permissions: contents: read` for jobs that checkout code (lint-and-test-units, test-integration), and `permissions: {}` for the notify job that only uses secrets.

