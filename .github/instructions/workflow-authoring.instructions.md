---
name: 'GitHub Actions Workflow Authoring'
description: 'Best practices for authoring GitHub Actions workflows, including SHA pinning, least-privilege permissions, and reusable workflow patterns'
applyTo: '**/*.yml,**/*.yaml'
---

# GitHub Actions Workflow Authoring Standards

## SHA Pinning (Mandatory)

- All `uses:` references to third-party actions MUST use full 40-character commit SHAs.
- **Never** use mutable tags like `v1`, `v2`, `@main`, or `@latest`.
- Add a comment with the tag/version for readability:
  ```yaml
  uses: actions/checkout@abc123...def456  # v4.1.0
  ```
- Use `REPLACE_WITH_*_SHA` placeholders for new actions until real SHAs are obtained.

## Permissions

- Set `permissions: {}` at the workflow level to deny all.
- Grant minimum required permissions per job, not per workflow.
- Common grants:
  - `contents: read` — for checkout
  - `checks: write` — for status checks
  - `pull-requests: write` — only if writing PR comments
- Document why each permission is needed with an inline comment.

## Reusable Workflows

- Use `workflow_call` trigger for shared workflows.
- Define typed inputs with descriptions.
- Use `secrets: inherit` sparingly — prefer explicit secret inputs.
- Callers should reference reusable workflows by pinned SHA.

## Concurrency

- Every workflow must have a `concurrency` block.
- Use `cancel-in-progress: false` for compliance workflows (don't cancel evidence collection).
- Pattern: `concurrency: { group: <workflow>-${{ github.ref }}, cancel-in-progress: false }`

## Evidence & Artifacts

- Upload audit evidence with `if: always()` so it's collected even on failure.
- Use descriptive artifact names including the run ID.
- Set `retention-days` per your audit policy (default: 90).
