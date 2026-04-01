# Dependency Review

Workflow file: [`../.github/workflows/_dependency-review.yml`](../.github/workflows/_dependency-review.yml)

## Purpose

Checks dependency changes in pull requests and fails when high-severity or
vulnerable dependency updates are introduced. Posts a summary comment on the PR.

## Usage

```yaml
jobs:
  dependency-review:
    # Only meaningful on public repos (or private repos with GHAS license)
    if: github.event.repository.private == false
    uses: atnplex/.github/.github/workflows/_dependency-review.yml@main
    secrets: inherit
    # Optional: override the runner
    # with:
    #   runner: ubuntu-latest
```

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in; no configuration needed)

## Notes

This is mainly useful for repositories where the GitHub dependency graph and
dependency review features are available and relevant. On private repos without
a GitHub Advanced Security (GHAS) license, this action will fail — guard it
with `if: github.event.repository.private == false` or enable GHAS first.
