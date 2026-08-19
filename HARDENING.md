<!-- markdownlint-disable -->

# Hardening Report: toshimaru--backlog-pr-link-action/v2.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--backlog-pr-link-action/v2.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced action tag is moved or compromised.

.github/workflows/check-diff.yml:
  - uses: actions/checkout@v4 (line 12)
  - uses: actions/setup-node@v4 (line 14)
  - uses: actions/github-script@v7 (line 23)

.github/workflows/ci.yml:
  - uses: actions/checkout@v4 (line 11)

.github/workflows/test.yml:
  - uses: actions/checkout@v4 (line 11)
  - uses: denoland/setup-deno@v1 (line 12)
  - uses: actions/setup-node@v4 (line 14)

All of these should be pinned to their full 40-character commit SHA (e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4).

Locations:

- `.github/workflows/check-diff.yml:12`
- `.github/workflows/check-diff.yml:14`
- `.github/workflows/check-diff.yml:23`
- `.github/workflows/ci.yml:11`
- `.github/workflows/test.yml:11`
- `.github/workflows/test.yml:12`
- `.github/workflows/test.yml:14`

### missing-permissions (severity: medium)

The workflow file .github/workflows/ci.yml has no top-level `permissions:` block and no job-level `permissions:` block on any of its jobs. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary. A minimal permissions block (e.g. `permissions: {}` or specific scopes like `contents: read`) should be added. This is especially important because the workflow is triggered by `pull_request_target`, which runs with write access to the base repository.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full 40-character commit SHAs: actions/checkout@v4 → 11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → 49933ea5288caeca8642d1e84afbd3f7d6820020, actions/github-script@v7 → f28e40c7f34bde8b3046d885e986cb6290c5673b, denoland/setup-deno@v1 → 11b63cf76cfcafb4e43f97b6cad24d8e8438f62d. Original tags preserved as inline comments. Added `permissions: {}` top-level block to ci.yml which lacked any permissions declaration despite being triggered by pull_request_target.

