# Dependency Review

Workflow file: [`.github/workflows/_dependency-review.yml`](../.github/workflows/_dependency-review.yml)

## Purpose

Checks dependency changes in pull requests and fails when high-severity or vulnerable dependency updates are introduced.

## Usage

```yaml
jobs:
  dependency-review:
    # Only useful for public repos or private repos with GHAS license
    if: github.event.repository.private == false
    uses: atnplex/.github/.github/workflows/_dependency-review.yml@main
    secrets: inherit
    # Optionally override the runner:
    # with:
    #   runner: 'ubuntu-latest'
```

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in)

## Notes

This is mainly useful for repositories where the GitHub dependency graph and dependency review features are available and relevant. Public repos qualify automatically; private repos require a GitHub Advanced Security (GHAS) license.
