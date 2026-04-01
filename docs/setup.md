# Setup

## Purpose

Explains how to configure `atnplex/.github` so that reusable workflows can be
shared across the organization, and provides a step-by-step onboarding
checklist for new org admins.

## Repository purpose

This repository stores:

- Reusable GitHub Actions workflows (called by other `atnplex` repos)
- Shared templates for new and existing repositories
- Automation documentation and onboarding guides

## Required GitHub settings

Go to:

- `atnplex/.github` → **Settings** → **Actions** → **General**

Configure:

1. **Allow GitHub Actions to run**
2. **Allow actions and reusable workflows** according to organization policy
3. **Access**: because this is a public repo, any repository can call these
   workflows. If you want to limit to `atnplex` org only, use an org-level
   Actions policy to restrict which repos can use workflow files from this repo.

## Organization allow list

If the organization uses "Allow specified actions and reusable workflows",
include at least:

```text
actions/*
autofix-ci/*
amannn/*
release-drafter/*
atnplex/*
```

Add more entries as additional third-party actions are adopted.

## Calling reusable workflows

Other repositories call these workflows with syntax like:

```yaml
uses: atnplex/.github/.github/workflows/_autofix.yml@main
```

Pin to a tag or commit SHA once the workflows stabilize for additional safety:

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
- [ ] Confirm `.github` Actions access setting is correct
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
