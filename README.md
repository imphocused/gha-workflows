# gha-workflows — Shared Reusable CI/CD Compliance Workflows

Centralized GitHub Actions workflows for homelab infrastructure repos. All repos call these reusable workflows to enforce consistent compliance checks and policy guardrails.

## Quick Start

### Calling the Reusable Workflow

In your repo's `.github/workflows/compliance-ci.yml`:

```yaml
name: Compliance CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

permissions: {}

jobs:
  compliance:
    uses: <your-org>/gha-workflows/.github/workflows/compliance-reusable.yml@REPLACE_WITH_WORKFLOW_SHA
    with:
      profile: terraform  # or: ansible, packer, scripts, octodns, websites
      retention_days: 90
```

### Supported Profiles

| Profile | Lint Steps | Security Steps |
|---------|-----------|----------------|
| `terraform` | `terraform fmt`, `terraform validate`, TFLint | TFSec, Checkov, detect-secrets |
| `ansible` | yamllint, ansible-lint | detect-secrets |
| `packer` | `packer fmt`, `packer validate` | detect-secrets |
| `scripts` | Ruff (Python), ShellCheck (Bash) | detect-secrets |
| `octodns` | yamllint, OctoDNS validate | detect-secrets |
| `websites` | HTMLHint | detect-secrets |

All profiles include **drift detection** (`git diff --exit-code`) and **audit evidence collection**.

## Policy Workflows

The repository now includes workflow policy guardrails:

- `.github/workflows/validate-workflows.yml` — workflow lint validation
- `.github/workflows/policy-enforce-sha-pinning.yml` — enforces immutable `uses:` refs
- `.github/workflows/policy-enforce-workflow-standards.yml` — enforces baseline workflow standards

These policy workflows are intended to become required checks once all `REPLACE_WITH_*_SHA` placeholders are resolved to 40-character commit SHAs.

## SHA Pinning

All action references in workflows use full 40-character commit SHAs. Placeholder format:

```
REPLACE_WITH_CHECKOUT_SHA        → actions/checkout
REPLACE_WITH_UPLOAD_ARTIFACT_SHA → actions/upload-artifact
REPLACE_WITH_TFSEC_SHA           → aquasecurity/tfsec-action
REPLACE_WITH_CHECKOV_SHA         → bridgecrewio/checkov-action
REPLACE_WITH_SHELLCHECK_SHA      → ludeeus/action-shellcheck
REPLACE_WITH_SETUP_TFLINT_SHA   → terraform-linters/setup-tflint
```

**To resolve placeholders**, find the latest release tag, then get its commit SHA:
```bash
git ls-remote --tags https://github.com/actions/checkout | grep 'v4' | tail -1
```

## Audit Evidence

Every workflow run produces an `audit-evidence/manifest.json` artifact containing:
- Repository, ref, SHA, and run metadata
- Pass/fail status for lint, security, and drift checks
- Timestamp for compliance records

Artifacts are retained for **90 days** by default. You can override with reusable input `retention_days`.

## Local Development

```bash
# Validate workflow syntax (requires actionlint)
actionlint .github/workflows/*.yml

# Check YAML syntax
yamllint .github/workflows/*.yml
```

## Repository Structure

```
.github/
├── workflows/
│   ├── compliance-reusable.yml                 # Main reusable workflow
│   ├── validate-workflows.yml                  # Workflow lint validation
│   ├── policy-enforce-sha-pinning.yml          # Immutable SHA enforcement
│   └── policy-enforce-workflow-standards.yml   # Baseline workflow standards
├── ISSUE_TEMPLATE/
│   ├── bug_report.yml
│   └── workflow_request.yml
├── instructions/
│   └── workflow-authoring.instructions.md
├── prompts/
│   └── new-profile.prompt.md
├── agents/
│   └── workflow-reviewer.agent.md
├── pull_request_template.md
├── dependabot.yml
└── copilot-instructions.md
AGENTS.md
CODEOWNERS
CONTRIBUTING.md
SECURITY.md
README.md
COMPLIANCE_CONTROLS.md
```
