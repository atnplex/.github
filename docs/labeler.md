# Labeler

Workflow file: [`.github/workflows/_labeler.yml`](../.github/workflows/_labeler.yml)

## Purpose

Automatically labels pull requests based on the files that changed.

## Usage

```yaml
jobs:
  labeler:
    uses: atnplex/.github/.github/workflows/_labeler.yml@main
    secrets: inherit
    # Optionally override the runner:
    # with:
    #   runner: 'ubuntu-latest'
```

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in)

## Config

Each repository must include `.github/labeler.yml`, typically copied from `templates/labeler.yml`.

## Notes

This is separate from Copilot or AI-generated commit/PR text:

- labeler assigns labels
- Copilot/GitKraken may suggest commit messages or PR text

They do not overlap in function.
