# Compliance Controls — gha-workflows

This document maps CI/CD controls to aspirational SOC2/ISO best-practice requirements.

## Control Mapping

| Control ID | Description | Implementation | Evidence |
|-----------|-------------|----------------|----------|
| CI-001 | All code changes pass automated lint checks | `compliance-reusable.yml` lint job | Workflow run logs + audit-evidence artifact |
| CI-002 | Security scanning on every change | `compliance-reusable.yml` security job | TFSec/Checkov/detect-secrets results |
| CI-003 | Drift detection prevents uncommitted changes | `compliance-reusable.yml` drift job | `git diff --exit-code` result |
| CI-004 | Audit evidence collected for every run | `compliance-reusable.yml` evidence job | `audit-evidence/manifest.json` artifact |
| CI-005 | GitHub Actions pinned to immutable SHAs | SHA pinning policy workflow | Policy workflow pass/fail |
| CI-006 | Dependency updates tracked automatically | Dependabot configuration | Dependabot PRs |
| CI-007 | Least-privilege workflow permissions | `permissions: {}` + per-job grants | Workflow file review |
| CI-008 | Concurrent runs controlled | `concurrency` blocks | Workflow configuration |

## Aspirational Alignment

- **SOC2 CC8.1** — Change management controls → CI-001, CI-002, CI-003
- **SOC2 CC6.1** — Logical access controls → CI-005, CI-007
- **ISO 27001 A.12.1.2** — Change management → CI-001, CI-003, CI-004
- **ISO 27001 A.14.2.2** — System change control → CI-002, CI-005

## Review Cadence

- **Weekly**: Review Dependabot PRs and merge approved updates.
- **Monthly**: Review audit evidence artifacts for completeness.
- **Quarterly**: Review control effectiveness and update this document.
