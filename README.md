# atnplex/.github

Shared reusable GitHub Actions workflows, templates, and documentation for the
`atnplex` organization.

## Repository Purpose

This repository provides:

- **Reusable CI/CD workflows** — called by any `atnplex` repo with a single
  `uses:` line
- **Templates** — copy-paste starting files for CI, labeler, release notes,
  pre-commit, editor config, and gitattributes
- **Documentation** — guides for workflows, runners, secrets, branch protection,
  setup, and onboarding

## Workflows

All workflows live in `.github/workflows/` and use a `_` prefix to signal that
they are reusable (not directly triggered on this repo).

| Workflow | Purpose | Docs |
|---|---|---|
| `_autofix.yml` | Auto-format and commit code fixes | [docs/autofix.md](docs/autofix.md) |
| `_dependency-review.yml` | Review dependency changes in PRs | [docs/dependency-review.md](docs/dependency-review.md) |
| `_labeler.yml` | Auto-label PRs by changed files | [docs/labeler.md](docs/labeler.md) |
| `_pr-title-check.yml` | Enforce Conventional Commits PR titles | [docs/pr-title-check.md](docs/pr-title-check.md) |
| `_release-drafter.yml` | Maintain automated draft releases | [docs/release-drafter.md](docs/release-drafter.md) |
| `_stale.yml` | Close stale issues and PRs | [docs/stale.md](docs/stale.md) |

## Quick Start

### 1. Call a workflow from a consumer repo

```yaml
# .github/workflows/ci.yml (in a consumer repo)
jobs:
  autofix:
    uses: atnplex/.github/.github/workflows/_autofix.yml@main
    secrets: inherit
```

Copy the full caller template from [`templates/repo-ci.yml`](templates/repo-ci.yml).

### 2. Override the runner (optional)

```yaml
jobs:
  autofix:
    uses: atnplex/.github/.github/workflows/_autofix.yml@main
    with:
      runner: ubuntu-latest  # force GitHub-hosted runner
    secrets: inherit
```

See [docs/runners.md](docs/runners.md) for the full runner strategy.

## Templates

| File | Use |
|---|---|
| `templates/repo-ci.yml` | Drop into `.github/workflows/ci.yml` in any repo |
| `templates/labeler.yml` | Drop into `.github/labeler.yml` |
| `templates/release-drafter.yml` | Drop into `.github/release-drafter.yml` |
| `templates/pre-commit-config.yaml` | Drop into `.pre-commit-config.yaml` |
| `templates/.editorconfig` | Drop into `.editorconfig` |
| `templates/.gitattributes` | Drop into `.gitattributes` |

## Documentation

| File | Content |
|---|---|
| [docs/workflows.md](docs/workflows.md) | All workflows — inputs, outputs, permissions |
| [docs/runners.md](docs/runners.md) | Runner strategy, `runner` input, outage handling |
| [docs/setup.md](docs/setup.md) | GitHub settings, org allow list, required secrets |
| [docs/secret-management.md](docs/secret-management.md) | Secret naming, config, incident response |
| [docs/security.md](docs/security.md) | Access model, CODEOWNERS, branch protection |
| [docs/branch-protection.md](docs/branch-protection.md) | Recommended branch protection rules |
| [docs/onboarding.md](docs/onboarding.md) | Step-by-step admin checklist |

## Conventions

- All `uses:` references are pinned to full commit SHAs with version comments
- Every workflow has explicit `permissions:`, `concurrency:`, and
  `timeout-minutes:` blocks
- New workflows must have a matching `docs/` file
- All changes require owner review via CODEOWNERS + branch protection
