# Stale

Workflow file: [`../.github/workflows/_stale.yml`](../.github/workflows/_stale.yml)

## Purpose

Marks inactive issues and PRs as stale and closes them after a grace period.

## Usage

```yaml
jobs:
  stale:
    if: github.event_name == 'schedule'
    uses: atnplex/.github/.github/workflows/_stale.yml@main
    secrets: inherit
    # Optional: override the runner
    # with:
    #   runner: ubuntu-latest
```

Typical trigger in the consumer caller workflow:

```yaml
on:
  schedule:
    - cron: "0 9 * * 1" # every Monday at 09:00 UTC
```

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in; no configuration needed)

## Notes

Exempt labels — items that should never go stale:

- `security`
- `blocked`
- `pinned`
- `do-not-merge`

Why use it: useful for keeping repositories tidy without manual triage.
