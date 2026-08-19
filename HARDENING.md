<!-- markdownlint-disable -->

# Hardening Report: toshimaru--backlog-pr-link-action/v2.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--backlog-pr-link-action/v2.2.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if a tag is moved or a repository is compromised.

.github/workflows/check-diff.yml:
  - actions/checkout@v6 (line 12)
  - actions/setup-node@v6 (line 14)
  - actions/github-script@v9 (line 23)

.github/workflows/ci.yml:
  - actions/checkout@v6 (line 12)

.github/workflows/test.yml:
  - actions/checkout@v6 (line 16)
  - denoland/setup-deno@v2 (line 17)
  - actions/setup-node@v6 (line 19)

All refs should be replaced with full 40-character hex commit SHAs (e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4).

Locations:

- `.github/workflows/check-diff.yml:12`
- `.github/workflows/check-diff.yml:14`
- `.github/workflows/check-diff.yml:23`
- `.github/workflows/ci.yml:12`
- `.github/workflows/test.yml:16`
- `.github/workflows/test.yml:17`
- `.github/workflows/test.yml:19`

### missing-permissions (severity: medium)

The workflow file ci.yml has no top-level `permissions:` key and its only job (`backlog-pr-link`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the default repository permissions (which may include write access to contents, packages, etc.), violating the principle of least privilege. A minimal `permissions: {}` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 7 unpinned action references across check-diff.yml, ci.yml, and test.yml to their full 40-character commit SHAs (actions/checkout@v6→d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6→249970729cb0ef3589644e2896645e5dc5ba9c38, actions/github-script@v9→3a2844b7e9c422d3c10d287c895573f7108da1b3, denoland/setup-deno@v2→22d081ff2d3a40755e97629de92e3bcbfa7cf2ed). Added `permissions: {}` top-level block to ci.yml to enforce least privilege since the workflow only uses repository secrets and a local action requiring no GitHub API permissions.

