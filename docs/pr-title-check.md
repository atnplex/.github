# PR Title Check

Workflow file: [`.github/workflows/_pr-title-check.yml`](../.github/workflows/_pr-title-check.yml)

## Purpose

Validates PR titles against a semantic convention such as:

- `feat: add x`
- `fix: resolve y`
- `docs: update z`

## Usage

```yaml
jobs:
  pr-title-check:
    uses: atnplex/.github/.github/workflows/_pr-title-check.yml@main
    secrets: inherit
    # Optionally override the runner:
    # with:
    #   runner: 'ubuntu-latest'
```

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in)

## Notes

This validates the PR title only, not commit messages. If this becomes too noisy for repos where PR titles are already well-formed, it can be disabled or softened with a separate `if:` condition in the caller.
