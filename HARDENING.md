<!-- markdownlint-disable -->

# Hardening Report: toshimaru--backlog-pr-link-action/v2.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--backlog-pr-link-action/v2.4.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the tag is moved.

.github/workflows/check-diff.yml:
  - uses: actions/checkout@v7
  - uses: actions/setup-node@v6
  - uses: actions/github-script@v9

.github/workflows/ci.yml:
  - uses: actions/checkout@v7

.github/workflows/test.yml:
  - uses: actions/checkout@v7
  - uses: denoland/setup-deno@v2
  - uses: actions/setup-node@v6

All of these should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-diff.yml:11`
- `.github/workflows/check-diff.yml:14`
- `.github/workflows/check-diff.yml:22`
- `.github/workflows/ci.yml:11`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:17`

### missing-permissions (severity: medium)

ci.yml has no top-level `permissions:` key and its only job (`backlog-pr-link`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal permissions block (e.g. `permissions: {}` or specific scopes) should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full SHAs: actions/checkout@v7 → 9c091bb21b7c1c1d1991bb908d89e4e9dddfe3e0, actions/setup-node@v6 → 249970729cb0ef3589644e2896645e5dc5ba9c38, actions/github-script@v9 → 3a2844b7e9c422d3c10d287c895573f7108da1b3, denoland/setup-deno@v2 → 22d081ff2d3a40755e97629de92e3bcbfa7cf2ed. Added `permissions: {}` to ci.yml to enforce least-privilege (the workflow uses only repository secrets and a local action, requiring no GitHub token scopes).

