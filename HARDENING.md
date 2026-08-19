<!-- markdownlint-disable -->

# Hardening Report: toshimaru--backlog-pr-link-action/v2.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--backlog-pr-link-action/v2.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across the workflow files use mutable version tags instead of full 40-character SHA commit digests, making the action vulnerable to supply-chain attacks if those tags are moved. Failing references:
- `.github/workflows/check-diff.yml`: `actions/checkout@v7`, `actions/setup-node@v6`, `actions/github-script@v9`
- `.github/workflows/ci.yml`: `actions/checkout@v7`
- `.github/workflows/test.yml`: `actions/checkout@v7`, `denoland/setup-deno@v2`, `actions/setup-node@v6`

Locations:

- `.github/workflows/check-diff.yml:10`
- `.github/workflows/check-diff.yml:13`
- `.github/workflows/check-diff.yml:21`
- `.github/workflows/ci.yml:11`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:17`

### missing-permissions (severity: medium)

`.github/workflows/ci.yml` has no top-level `permissions:` key and no job-level `permissions:` key on any of its jobs. Without explicit permissions, the workflow inherits the default repository permissions (which may include broad write access), violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 7 unpinned `uses:` references to full 40-character SHA digests with original tags preserved as comments: actions/checkout@v7→3d3c42e5, actions/setup-node@v6→249970729c, actions/github-script@v9→3a2844b7, denoland/setup-deno@v2→22d081ff. Added `permissions: contents: read` top-level block to ci.yml which had no permissions block.

