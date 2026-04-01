# Autofix

Workflow file: [`../.github/workflows/_autofix.yml`](../.github/workflows/_autofix.yml)

## Purpose

Automatically format and normalize repository contents, then commit those safe
fixes back to the branch using autofix.ci.

## Usage

```yaml
jobs:
  autofix:
    uses: atnplex/.github/.github/workflows/_autofix.yml@main
    secrets: inherit
    # Optional: override the runner
    # with:
    #   runner: ubuntu-latest
```

## What it handles

This workflow is designed to cover common repository hygiene and formatting concerns:

- Tabs vs spaces normalization through formatters/editor config
- LF/CRLF normalization through `.gitattributes`
- Trimming trailing whitespace
- Final newline enforcement
- YAML, JSON, Markdown, shell, Python, Go, and JS/TS formatting
- Basic whitespace and syntax hygiene through pre-commit hooks

## Recommended companion files

Use these files in each repository:

- `templates/pre-commit-config.yaml`
- `templates/.editorconfig`
- `templates/.gitattributes`

## Required Secrets / Permissions

- `GITHUB_TOKEN` (built-in; no configuration needed)
- `ATNPLEX_ACTIONS_TOKEN` (optional org secret; only needed if the built-in
  token cannot push to protected branches)

## Notes

This workflow is intentionally focused on safe, mechanical fixes. It should not
be treated as a replacement for full test or build workflows.
