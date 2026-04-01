# Secret Management

## Purpose

Documents all secrets required by the shared reusable workflows in
`atnplex/.github`, where they must be configured, their naming conventions,
and what to do if a secret is accidentally committed.

## Required Secrets

The following secrets must exist at the **organization level** so that all
`atnplex` repositories can use the shared workflows without additional
per-repo configuration.

| Secret name | Scope | Required by | Description |
|---|---|---|---|
| `GITHUB_TOKEN` | Built-in (automatic) | All workflows | GitHub-provided per-run token; no configuration needed |
| `ATNPLEX_ACTIONS_TOKEN` | Org secret | `_autofix.yml` (optional) | PAT with `repo` scope if autofix.ci needs to push to protected branches. Only needed if the built-in `GITHUB_TOKEN` lacks write access. |

> **Note:** The built-in `GITHUB_TOKEN` is sufficient for most workflows.
> `ATNPLEX_ACTIONS_TOKEN` is only required if branch protection rules prevent
> the default token from pushing commits (e.g., when "Restrict pushes" is
> enforced for the bot identity).

## Naming Convention

All org-level secrets used by `atnplex` workflows must be prefixed with
`ATNPLEX_` to distinguish them from third-party or repo-level secrets:

```
ATNPLEX_<PURPOSE>
```

Examples:
- `ATNPLEX_ACTIONS_TOKEN` — personal access token for Actions automation
- `ATNPLEX_DEPLOY_KEY` — SSH deploy key for deployment workflows

## Where to Configure Secrets

### Organization-level secrets (preferred)

Go to: **GitHub → atnplex org → Settings → Secrets and variables → Actions**

- Set **Repository access** to: "All repositories" or specifically allowlist
  the repositories that consume these workflows.
- Organization secrets are automatically available to workflows that use
  `secrets: inherit` in their caller files.

### Repository-level secrets (when necessary)

Go to: **GitHub → [repo] → Settings → Secrets and variables → Actions**

Use repository secrets only for credentials that are specific to a single
repository (e.g., a deployment key for that repo's hosting environment).

## Usage

In a consumer repo's caller workflow, secrets are passed with:

```yaml
jobs:
  autofix:
    uses: atnplex/.github/.github/workflows/_autofix.yml@main
    secrets: inherit  # passes all available secrets from the caller context
```

The `secrets: inherit` pattern forwards all secrets available in the calling
workflow to the reusable workflow. No explicit secret mapping is required as
long as org-level secrets are configured correctly.

## What to Do if a Secret Is Accidentally Committed

If a secret, token, API key, or credential is committed to any repository:

1. **Revoke the secret immediately.** Do not wait. Go to the service that issued
   the credential and revoke or rotate it before doing anything else.
2. **Remove the secret from git history.** Use `git filter-repo` or the
   [GitHub support process](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)
   to purge the secret from all commits.
3. **Force-push the cleaned history** to all affected branches after removal.
4. **Notify all org admins** via a private channel immediately.
5. **File an incident report** in a private issue or internal document,
   documenting: what was exposed, for how long, who had access, and what
   remediation steps were taken.
6. **Review secret scanning alerts** in the repository's Security tab to
   confirm GitHub has detected and flagged the exposure.

> The `detect-secrets` pre-commit hook in `templates/pre-commit-config.yaml`
> is the first line of defense. Always run pre-commit locally before pushing.

## Required Secrets / Permissions

This document itself requires no secrets. Configuring org secrets requires
an org **owner** role.

## Notes

- Never store secrets in workflow YAML files, even as comments or examples.
- Always use `${{ secrets.NAME }}` references in workflow files.
- The `.gitignore` in this repo explicitly ignores common secret file patterns
  (`.env`, `*.pem`, `*.key`, etc.).
