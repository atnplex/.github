# Workflow Overview

## Purpose

Lists every reusable workflow in `.github/workflows/` with its filename,
trigger, purpose, inputs, and outputs.

## Usage

Call these workflows from any `atnplex` repository's CI caller file
(see `templates/repo-ci.yml`):

```yaml
jobs:
  autofix:
    uses: atnplex/.github/.github/workflows/_autofix.yml@main
    secrets: inherit
```

To force a specific runner (e.g., during a self-hosted runner outage):

```yaml
jobs:
  autofix:
    uses: atnplex/.github/.github/workflows/_autofix.yml@main
    with:
      runner: ubuntu-latest
    secrets: inherit
```

## Workflows

### `_autofix.yml`

| Field | Value |
|---|---|
| **Trigger** | `workflow_call` |
| **Purpose** | Auto-format and normalize repository contents; commit safe fixes using autofix.ci |
| **Runner** | Self-hosted (private atnplex repos) or ubuntu-latest; overridable via `runner` input |
| **Permissions** | `contents: write` |
| **Timeout** | 30 minutes |
| **Inputs** | `runner` (optional string, default: auto) |
| **Outputs** | None |
| **Secrets** | `GITHUB_TOKEN` (built-in); optionally `ATNPLEX_ACTIONS_TOKEN` |

Handles: pre-commit hooks, Prettier, Ruff, shfmt, gofmt. Commits fixes
automatically via autofix.ci.

---

### `_dependency-review.yml`

| Field | Value |
|---|---|
| **Trigger** | `workflow_call` |
| **Purpose** | Review dependency changes in PRs; fail on high-severity vulnerabilities |
| **Runner** | Self-hosted (private atnplex repos) or ubuntu-latest; overridable via `runner` input |
| **Permissions** | `contents: read`, `pull-requests: write` |
| **Timeout** | 10 minutes |
| **Inputs** | `runner` (optional string, default: auto) |
| **Outputs** | None |
| **Secrets** | `GITHUB_TOKEN` (built-in) |

Posts a summary comment on the PR with all dependency changes and their
risk severity.

---

### `_labeler.yml`

| Field | Value |
|---|---|
| **Trigger** | `workflow_call` |
| **Purpose** | Automatically apply labels to PRs based on changed files |
| **Runner** | Self-hosted (private atnplex repos) or ubuntu-latest; overridable via `runner` input |
| **Permissions** | `contents: read`, `pull-requests: write` |
| **Timeout** | 10 minutes |
| **Inputs** | `runner` (optional string, default: auto) |
| **Outputs** | None |
| **Secrets** | `GITHUB_TOKEN` (built-in) |

Reads the calling repo's `.github/labeler.yml` config. See
`templates/labeler.yml` for a starting configuration.

---

### `_pr-title-check.yml`

| Field | Value |
|---|---|
| **Trigger** | `workflow_call` |
| **Purpose** | Validate PR titles against Conventional Commits format |
| **Runner** | Self-hosted (private atnplex repos) or ubuntu-latest; overridable via `runner` input |
| **Permissions** | `pull-requests: read` |
| **Timeout** | 10 minutes |
| **Inputs** | `runner` (optional string, default: auto) |
| **Outputs** | None |
| **Secrets** | `GITHUB_TOKEN` (built-in) |

Accepted types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`,
`build`, `ci`, `chore`, `revert`. Scope is optional.

---

### `_release-drafter.yml`

| Field | Value |
|---|---|
| **Trigger** | `workflow_call` |
| **Purpose** | Maintain a draft release with categorized PR notes |
| **Runner** | Self-hosted (private atnplex repos) or ubuntu-latest; overridable via `runner` input |
| **Permissions** | `contents: write`, `pull-requests: read` |
| **Timeout** | 10 minutes |
| **Inputs** | `runner` (optional string, default: auto) |
| **Outputs** | None |
| **Secrets** | `GITHUB_TOKEN` (built-in) |

Reads the calling repo's `.github/release-drafter.yml` config. See
`templates/release-drafter.yml` for a starting configuration.

---

### `_stale.yml`

| Field | Value |
|---|---|
| **Trigger** | `workflow_call` |
| **Purpose** | Mark and close stale issues and PRs |
| **Runner** | Self-hosted (private atnplex repos) or ubuntu-latest; overridable via `runner` input |
| **Permissions** | `issues: write`, `pull-requests: write` |
| **Timeout** | 10 minutes |
| **Inputs** | `runner` (optional string, default: auto) |
| **Outputs** | None |
| **Secrets** | `GITHUB_TOKEN` (built-in) |

Issues go stale after 60 days; PRs after 30 days. Both close 7 days after
being marked stale. Exempt labels: `pinned`, `security`, `blocked`,
`do-not-merge`.

---

## Runner Input

All workflows accept an optional `runner` input. When omitted, each workflow
applies the standard atnplex runner expression:

- Private repos owned by `atnplex` → `["self-hosted", "linux"]`
- Public repos or external orgs → `ubuntu-latest`

To override (e.g., force GitHub-hosted runners during a self-hosted outage):

```yaml
with:
  runner: ubuntu-latest
```

See [docs/runners.md](./runners.md) for the full runner strategy.

## Required Secrets / Permissions

All workflows rely on the built-in `GITHUB_TOKEN`. No additional org secrets
are required unless branch protection prevents the built-in token from pushing
(see `docs/secret-management.md`).

## Notes

- All workflows use `concurrency:` to prevent redundant parallel runs
- All `uses:` references are pinned to full commit SHAs with version comments
- The standard runner expression is consistent across all workflows
