<!-- markdownlint-disable -->

# Hardening Report: toshimaru--backlog-pr-link-action--/v2.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **toshimaru--backlog-pr-link-action--/v2.4.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use mutable tag-based refs instead of pinned 40-character SHA commit hashes, making them vulnerable to supply-chain attacks. Failing references: check-diff.yml uses actions/checkout@v7, actions/setup-node@v6, actions/github-script@v9; ci.yml uses actions/checkout@v7; test.yml uses actions/checkout@v7, denoland/setup-deno@v2, actions/setup-node@v6.

Locations:

- `.github/workflows/check-diff.yml:11`
- `.github/workflows/check-diff.yml:13`
- `.github/workflows/check-diff.yml:22`
- `.github/workflows/ci.yml:10`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:15`
- `.github/workflows/test.yml:17`

### missing-permissions (severity: medium)

ci.yml has no top-level permissions key and no job-level permissions key on any job. Without explicit permissions, the workflow inherits the default (potentially broad) permissions from the repository settings.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across 3 workflow files by pinning to full 40-character commit SHAs (with tag comments for readability): actions/checkout@v7→9c091bb, actions/setup-node@v6→48b55a0, actions/github-script@v9→3a2844b, denoland/setup-deno@v2→22d081f. Added `permissions: {}` top-level block to ci.yml to address the missing-permissions finding.

