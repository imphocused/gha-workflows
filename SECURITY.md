# Security Policy

## Supported scope

This repository contains shared CI/CD workflows used by multiple repos.
Security issues here may affect many pipelines.

## Reporting a vulnerability

Please report privately to:
- security@your-org.example
- or your internal security intake process

Do NOT open public issues for active vulnerabilities.

## Security standards

- Immutable SHA pinning for third-party actions
- Least-privilege permissions
- No long-lived secrets in workflows
- Prefer OIDC over static cloud credentials
- Explicit audit evidence retention

## Response expectations

- Initial triage: within 2 business days
- Severity classification + remediation plan: ASAP based on impact
