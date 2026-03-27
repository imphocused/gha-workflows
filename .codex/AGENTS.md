# Codex Agent Instructions — gha-workflows

> See `AGENTS.md` in the repo root and `.github/copilot-instructions.md` for full guidance.

This repo contains shared reusable GitHub Actions workflows for homelab CI/CD compliance.

## Key Rules
- All actions pinned to full commit SHAs (use `REPLACE_WITH_*_SHA` placeholders).
- Workflow-level `permissions: {}`, minimum grants per job.
- Concurrency controls on every workflow.
- Audit evidence uploaded with `if: always()`.
