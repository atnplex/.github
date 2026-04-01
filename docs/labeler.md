# Labeler

Workflow file: [`../.github/workflows/_labeler.yml`](../.github/workflows/_labeler.yml)

## Purpose

Automatically labels pull requests based on the files that changed.

## Usage

```yaml
jobs:
  labeler:
    uses: atnplex/.github/.github/workflows/_labeler.yml@main
    secrets: inherit
    # Optional: override the runner
    # with:
    #   runner: ubuntu-latest
```

## Config

Each repository should include `.github/labeler.yml`, typically copied from
`templates/labeler.yml`.

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in; no configuration needed)

## Notes

This is separate from Copilot or AI-generated commit/PR text:

- labeler assigns labels based on changed files
- Copilot/GitKraken may suggest commit messages or PR text

They do not overlap in function.
