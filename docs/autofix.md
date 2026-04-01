# Autofix

Workflow file: [`.github/workflows/_autofix.yml`](../.github/workflows/_autofix.yml)

## Purpose

Automatically format and normalize repository contents, then commit those safe fixes back to the branch using autofix.ci.

## What it handles

This workflow covers common repository hygiene and formatting concerns:

- tabs vs spaces normalization through formatters/editor config
- LF/CRLF normalization through `.gitattributes`
- trimming trailing whitespace
- final newline enforcement
- YAML, JSON, Markdown, shell, Python, Go, and JS/TS formatting
- basic whitespace and syntax hygiene through pre-commit hooks

## Usage

```yaml
jobs:
  autofix:
    uses: atnplex/.github/.github/workflows/_autofix.yml@main
    secrets: inherit
    # Optionally override the runner:
    # with:
    #   runner: 'ubuntu-latest'
```

## Recommended companion files

Use these files in each repository:

- `templates/pre-commit-config.yaml`
- `templates/.editorconfig`
- `templates/.gitattributes`

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in) — sufficient for most repos
- `ATNPLEX_ACTIONS_TOKEN` (org secret, optional) — only needed if the built-in token cannot push to a protected branch

## Notes

This workflow is intentionally focused on safe, mechanical fixes. It should not be treated as a replacement for full test or build workflows.
