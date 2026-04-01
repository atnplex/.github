# Release Drafter

Workflow file: [`.github/workflows/_release-drafter.yml`](../.github/workflows/_release-drafter.yml)

## Purpose

Builds and updates draft release notes automatically from merged pull requests and labels.

## Usage

```yaml
jobs:
  release-drafter:
    if: github.event_name == 'push' && (github.ref == 'refs/heads/main' || github.ref == 'refs/heads/master')
    uses: atnplex/.github/.github/workflows/_release-drafter.yml@main
    secrets: inherit
    # Optionally override the runner:
    # with:
    #   runner: 'ubuntu-latest'
```

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in)

## Config

Each repository must include `.github/release-drafter.yml`, usually copied from `templates/release-drafter.yml`.
