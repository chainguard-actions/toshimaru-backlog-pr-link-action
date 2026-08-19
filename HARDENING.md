<!-- markdownlint-disable -->

# Hardening Report: toshimaru--backlog-pr-link-action/v2.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--backlog-pr-link-action/v2.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use tag-based (mutable) `uses:` references instead of pinned full 40-character SHA commit hashes, making the workflows vulnerable to supply-chain attacks if the referenced action tags are moved or compromised.

- `.github/workflows/check-diff.yml`: `actions/checkout@v4`, `actions/setup-node@v4`, `actions/github-script@v7`
- `.github/workflows/ci.yml`: `actions/checkout@v4`
- `.github/workflows/test.yml`: `actions/checkout@v4`, `denoland/setup-deno@v1`, `actions/setup-node@v4`

All should be replaced with their full SHA digests, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-diff.yml:11`
- `.github/workflows/check-diff.yml:13`
- `.github/workflows/check-diff.yml:20`
- `.github/workflows/ci.yml:11`
- `.github/workflows/test.yml:9`
- `.github/workflows/test.yml:10`
- `.github/workflows/test.yml:12`

### missing-permissions (severity: medium)

`.github/workflows/ci.yml` has no top-level `permissions:` key and none of its jobs define a `permissions:` block. Without explicit permissions, the workflow inherits the default repository permissions (which may include `write` access to contents and other scopes), violating the principle of least privilege. A `permissions:` block with minimal required scopes should be added at the top level or on each job.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across 3 workflow files by replacing tag-based references with full 40-character SHA commit hashes (preserving tags as comments): actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, actions/github-script@v7 → @f28e40c7f34bde8b3046d885e986cb6290c5673b, denoland/setup-deno@v1 → @11b63cf76cfcafb4e43f97b6cad24d8e8438f62d. Added `permissions: {}` top-level block to ci.yml to enforce least privilege (the workflow only uses secrets and a local action, requiring no GitHub token scopes).

