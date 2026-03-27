---
description: 'Add a new tool profile to the reusable compliance workflow'
name: 'new-profile'
agent: 'agent'
tools: ['codebase', 'editFiles', 'terminalCommand']
argument-hint: 'Name of the new profile (e.g., "docker", "helm")'
---

# Add New Compliance Profile

Add a new tool profile to the reusable compliance workflow at `.github/workflows/compliance-reusable.yml`.

## Steps

1. Read the current `compliance-reusable.yml` to understand the existing profile structure.
2. For the new profile `${input:profileName}`:
   - Add lint steps specific to the tool (format check, validation).
   - Add security scanning steps if applicable.
   - Ensure all new actions use `REPLACE_WITH_*_SHA` placeholders.
   - Add `if: inputs.profile == '${input:profileName}'` conditions.
3. Update the `profile` input description to include the new profile name.
4. Update the `AGENTS.md` and `README.md` to document the new profile.

## Requirements

- Follow existing patterns for step naming and grouping.
- Use `permissions: { contents: read }` minimum.
- Add inline comments explaining what each step validates.
- Use `REPLACE_WITH_*_SHA` for any new action references.
