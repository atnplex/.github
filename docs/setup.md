# Setup

## Purpose

Explains how to configure `atnplex/.github` so that reusable workflows
can be shared across the organization, and provides a step-by-step onboarding
checklist for new org admins.

## Repository purpose

This repository stores:

- reusable GitHub Actions workflows (called by other `atnplex` repos)
- shared templates for new and existing repositories
- workflow documentation and conventions

## Required GitHub settings

Go to:

- `atnplex/.github` → **Settings** → **Actions** → **General**

Configure:

1. **Allow GitHub Actions to run**
2. **Allow actions and reusable workflows** according to organization policy
3. **Access**: because this repo is **public**, any repository (inside or outside
   the org) can call the reusable workflows. If you want to restrict callers to
   `atnplex`-owned repos only, add `if: github.repository_owner == 'atnplex'`
   conditions inside each workflow, or enforce this at the org Actions policy level.

## Organization allow list

If the organization uses "Allow specified actions and reusable workflows",
include at least:

```text
actions/*
autofix-ci/*
amannn/*
release-drafter/*
atnplex/*
anguy079/*
atngit2/*
zhiq7/*
```

Add more entries later if additional third-party actions are adopted.

## Calling reusable workflows

Other repositories call these workflows with:

```yaml
uses: atnplex/.github/.github/workflows/_autofix.yml@main
```

Pin to a tag or commit SHA once the workflows stabilize for production use:

```yaml
uses: atnplex/.github/.github/workflows/_autofix.yml@v1.0.0
```

## Required secrets

The following secrets must be configured at the **organization level**
(Settings → Secrets and variables → Actions):

| Secret | Required by | Notes |
|---|---|---|
| `GITHUB_TOKEN` | All workflows | Built-in; no configuration needed |
| `ATNPLEX_ACTIONS_TOKEN` | `_autofix.yml` (optional) | PAT with `repo` scope; only needed if the built-in token cannot push to protected branches |

See [`docs/secret-management.md`](./secret-management.md) for full details on
secret configuration, naming conventions, and incident response.

## New admin onboarding checklist

See [`docs/onboarding.md`](./onboarding.md) for the complete per-admin checklist.

Quick summary for new admins:

- [ ] Confirm org Owner role at github.com/orgs/atnplex/people
- [ ] Confirm org secrets are configured and accessible
- [ ] Confirm self-hosted runners are visible and online
- [ ] Confirm `.github` Actions settings are correctly configured
- [ ] Successfully trigger a test workflow dispatch from a consumer repo
- [ ] Confirm branch protection and CODEOWNERS are active on `main`
- [ ] Verify no secrets are present in any committed file

## Recommended companion files in consumer repos

Each repo should typically have:

- `.github/workflows/ci.yml` — copy from `templates/repo-ci.yml`
- `.github/labeler.yml` — copy from `templates/labeler.yml`
- `.github/release-drafter.yml` — copy from `templates/release-drafter.yml`
- `.pre-commit-config.yaml` — copy from `templates/pre-commit-config.yaml`
- `.editorconfig` — copy from `templates/.editorconfig`
- `.gitattributes` — copy from `templates/.gitattributes`
