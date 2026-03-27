# Contributing to gha-workflows

## Goals

Keep reusable workflows:
- secure-by-default
- backward-compatible when possible
- auditable and deterministic

## Change process

1. Open a PR with:
   - summary of change
   - security impact
   - compatibility impact
   - rollback plan
2. Ensure policy workflows pass.
3. Require CODEOWNERS approval.
4. Merge only after successful checks.

## Required PR checklist

- [ ] All `uses:` refs are SHA-pinned.
- [ ] Workflow has `permissions: {}` at top-level.
- [ ] Job permissions are least-privilege.
- [ ] Concurrency is configured.
- [ ] Audit artifact behavior remains intact.
- [ ] README/docs updated if interface changed.
- [ ] Consumer impact documented.

## Breaking changes

If inputs/outputs/behavior change:
- bump release tag
- add migration notes
- provide caller workflow update example
