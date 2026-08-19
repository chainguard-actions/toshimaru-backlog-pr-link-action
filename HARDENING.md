<!-- markdownlint-disable -->

# Hardening Report: toshimaru--backlog-pr-link-action/v2.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--backlog-pr-link-action/v2.1.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if a tag is moved or a dependency is compromised.

- .github/workflows/check-diff.yml: `actions/checkout@v4` (line 12), `actions/setup-node@v4` (line 14), `actions/github-script@v7` (line 23)
- .github/workflows/ci.yml: `actions/checkout@v4` (line 11)
- .github/workflows/test.yml: `actions/checkout@v4` (line 15), `denoland/setup-deno@v1` (line 16), `actions/setup-node@v4` (line 18)

Locations:

- `.github/workflows/check-diff.yml:12`
- `.github/workflows/check-diff.yml:14`
- `.github/workflows/check-diff.yml:23`
- `.github/workflows/ci.yml:11`
- `.github/workflows/test.yml:15`
- `.github/workflows/test.yml:16`
- `.github/workflows/test.yml:18`

### missing-permissions (severity: medium)

The workflow file .github/workflows/ci.yml has no top-level `permissions:` key and its only job (`backlog-pr-link`) also has no job-level `permissions:` key. This means the workflow runs with the default (broad) token permissions. This is especially risky because the workflow is triggered by `pull_request_target`, which runs with write access to the repository by default.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across 3 workflow files by replacing mutable version tags with full 40-character commit SHAs (preserving tags as comments). Added a `permissions: contents: read` block to ci.yml which was missing permissions entirely and triggered by pull_request_target (which runs with write access by default). Specific SHAs pinned: actions/checkout@v4→34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4→49933ea5288caeca8642d1e84afbd3f7d6820020, actions/github-script@v7→f28e40c7f34bde8b3046d885e986cb6290c5673b, denoland/setup-deno@v1→11b63cf76cfcafb4e43f97b6cad24d8e8438f62d.

