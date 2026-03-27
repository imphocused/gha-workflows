# gha-workflows — Agent Instructions

This repository contains **shared reusable GitHub Actions workflows** consumed by all homelab repos.

## Repository Purpose

- Single source of truth for CI/CD compliance workflows.
- Provides a reusable `compliance-reusable.yml` workflow callable via `workflow_call`.
- Supports profiles: `terraform`, `ansible`, `packer`, `scripts`, `octodns`, `websites`.

## Conventions

- All actions must use **pinned SHAs**, never mutable tags (`v1`, `latest`).
- Workflow-level `permissions: {}` (deny all), then grant minimum per job.
- Every workflow must include a `concurrency` block to prevent parallel runs on the same ref.
- Audit evidence must be uploaded as artifacts with `if: always()`.
- Drift detection via `git diff --exit-code` must fail the workflow if uncommitted changes exist.

## When Editing Workflows

- Use `workflow_call` inputs with clear descriptions and type constraints.
- Add inline comments explaining non-obvious permission grants or conditional logic.
- Test changes by creating a caller workflow in a test repo before merging.
- Never use `pull_request_target` without explicit security review.
- Keep reusable workflows focused — one workflow per compliance concern.

## File Structure

```
.github/workflows/
  compliance-reusable.yml    # Main reusable compliance workflow
```

## Security

- No secrets in workflow files — use `secrets: inherit` or explicit secret inputs.
- Minimize `contents: write` and `pull-requests: write` grants.
- Pin all third-party actions to full commit SHAs.
