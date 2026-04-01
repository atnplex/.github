# Stale

Workflow file: [`.github/workflows/_stale.yml`](../.github/workflows/_stale.yml)

## Purpose

Marks inactive issues and PRs as stale and closes them after a grace period.

## Usage

```yaml
jobs:
  stale:
    if: github.event_name == 'schedule'
    uses: atnplex/.github/.github/workflows/_stale.yml@main
    secrets: inherit
    # Optionally override the runner:
    # with:
    #   runner: 'ubuntu-latest'
```

The caller workflow should include a schedule trigger, e.g.:

```yaml
on:
  schedule:
    - cron: "0 9 * * 1" # every Monday at 09:00 UTC
```

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in)

## Why use it

Keeps repositories tidy without manual triage.

## Notes

Exempt labels should be used for items that should never go stale:

- `security`
- `blocked`
- `pinned`
- `do-not-merge`
