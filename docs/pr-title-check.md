# PR Title Check

Workflow file: [`../.github/workflows/_pr-title-check.yml`](../.github/workflows/_pr-title-check.yml)

## Purpose

Validates PR titles against the Conventional Commits format:

- `feat: add x`
- `fix: resolve y`
- `docs: update z`

## Usage

```yaml
jobs:
  pr-title-check:
    uses: atnplex/.github/.github/workflows/_pr-title-check.yml@main
    secrets: inherit
    # Optional: override the runner
    # with:
    #   runner: ubuntu-latest
```

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in; no configuration needed)

## Notes

This is separate from commit message generation tools. It validates the PR title
only, not individual commit messages.

If this becomes too noisy for repos where PR titles are already generated well,
it can be disabled or softened later.
