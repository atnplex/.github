# Copilot Instructions — atnplex/.github

## Repository Context

This is the `atnplex/.github` repository. It is a **public** repository that
provides:

- Reusable GitHub Actions workflows for all `atnplex` repositories
- Templates for new and existing repositories
- Documentation and onboarding guides for org admins

All reusable workflows live in `.github/workflows/` and are named with a `_`
prefix. Consumer repos call them with:

```yaml
uses: atnplex/.github/.github/workflows/_autofix.yml@main
```

## Conventions

- **Action pin format**: all `uses:` references must use a full commit SHA with
  a version comment, e.g.:
  ```yaml
  uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
  ```
- **Runner input**: all reusable workflows expose an optional `runner` input.
  When omitted, the standard expression selects self-hosted runners for private
  `atnplex` repos and `ubuntu-latest` for everything else.
- **Permissions**: every workflow must have an explicit `permissions:` block at
  the top-level. Use the minimum necessary permissions.
- **Concurrency**: every workflow must have a `concurrency:` block using
  `${{ github.repository }}-${{ github.ref }}`.
- **Timeout**: every job must have `timeout-minutes:` set.
- **Docs**: every workflow must have a corresponding doc file in `docs/`
  with matching filename (e.g., `_autofix.yml` → `docs/autofix.md`).

## Workflow File Template

New reusable workflows should follow this structure:

```yaml
# .github/workflows/_example.yml
#
# What it does:  One-line description
# When it runs:  Called by consumer repos via `workflow_call` on [event]
# Permissions:   [list permissions]

name: _example

on:
  workflow_call:
    inputs:
      runner:
        description: |
          Runner label(s) to use for this job. Accepts a string (single label)
          or a JSON array string for multiple labels, e.g. '["self-hosted","linux"]'.
          Defaults to the standard atnplex runner expression.
        type: string
        required: false
        default: ""

permissions:
  contents: read

concurrency:
  group: example-${{ github.repository }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  example:
    name: Example Job
    runs-on: >
      ${{ inputs.runner != '' && inputs.runner
        || (github.event.repository.private == true && github.repository_owner == 'atnplex')
          && fromJSON('["self-hosted","linux"]')
        || 'ubuntu-latest' }}
    timeout-minutes: 10

    steps:
      - name: Checkout
        uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
        with:
          persist-credentials: false
```

## Out of Scope

- Do not add deployment workflows, build pipelines, or application-specific CI
  to this repository.
- Do not add org-sensitive information (internal hostnames, credentials, etc.).
  This repository is public.
- Do not store secrets in any committed file.
