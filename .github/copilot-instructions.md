# gha-workflows — Copilot Instructions

This is a shared reusable GitHub Actions workflow repository for homelab CI/CD compliance.

## Key Rules

- All GitHub Actions must be pinned to full commit SHAs (40-char hex), never tags.
- Workflow-level `permissions: {}` then grant minimum per job.
- Every reusable workflow uses `workflow_call` with typed inputs.
- Audit evidence artifacts uploaded with `if: always()`.
- Concurrency controls on every workflow.

## Profiles

The reusable workflow supports these profiles via the `profile` input:
`terraform`, `ansible`, `packer`, `scripts`, `octodns`, `websites`

Each profile runs tool-specific linting, security scanning, and drift detection.
