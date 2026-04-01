# Release Drafter

Workflow file: [`../.github/workflows/_release-drafter.yml`](../.github/workflows/_release-drafter.yml)

## Purpose

Builds and updates draft release notes automatically from merged pull requests
and their labels.

## Usage

```yaml
jobs:
  release-drafter:
    if: github.event_name == 'push' && (github.ref == 'refs/heads/main' || github.ref == 'refs/heads/master')
    uses: atnplex/.github/.github/workflows/_release-drafter.yml@main
    secrets: inherit
    # Optional: override the runner
    # with:
    #   runner: ubuntu-latest
```

## Config

Each repository should include `.github/release-drafter.yml`, usually copied
from `templates/release-drafter.yml`.

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in; no configuration needed)

## Notes

The draft release is updated on every push to `main`/`master`. To publish, go
to the Releases page and click **Publish release**.
