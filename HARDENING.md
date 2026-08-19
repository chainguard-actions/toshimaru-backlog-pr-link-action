<!-- markdownlint-disable -->

# Hardening Report: toshimaru--backlog-pr-link-action/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--backlog-pr-link-action/v2.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across the workflow files use mutable version tags instead of pinned 40-character commit SHAs, making the action vulnerable to supply-chain attacks if those tags are moved.

.github/workflows/ci.yml:
  - uses: actions/checkout@v6

.github/workflows/test.yml:
  - uses: actions/checkout@v6
  - uses: denoland/setup-deno@v2
  - uses: actions/setup-node@v6

.github/workflows/check-diff.yml:
  - uses: actions/checkout@v6
  - uses: actions/setup-node@v6
  - uses: actions/github-script@v8

Locations:

- `.github/workflows/ci.yml:12`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:15`
- `.github/workflows/test.yml:17`
- `.github/workflows/check-diff.yml:10`
- `.github/workflows/check-diff.yml:13`
- `.github/workflows/check-diff.yml:20`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/ci.yml` has no top-level `permissions:` key and its only job (`backlog-pr-link`) also has no job-level `permissions:` key. This means the workflow runs with the default (broad) token permissions. This is especially risky because the workflow is triggered by `pull_request_target`, which runs with write access to the base repository.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned `uses:` references across ci.yml, test.yml, and check-diff.yml by replacing mutable version tags with full 40-character commit SHAs (preserving tags as comments). Added `permissions: contents: read` top-level block to ci.yml which was missing permissions and triggered by the high-risk `pull_request_target` event. The other two workflow files already had permissions blocks which were preserved.

