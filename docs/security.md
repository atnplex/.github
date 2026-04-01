# Security

## Purpose

Documents the security posture of `atnplex/.github`: secret management,
CODEOWNERS enforcement, branch protection, and considerations for a
publicly-readable workflow repository.

## Public Repo Considerations

`atnplex/.github` is a **public repository**. Its reusable workflows are
readable by anyone on GitHub. This is intentional — the workflows contain no
secrets or sensitive logic.

However, because the repo is public:

- **Any repository can reference these workflows.** If you need to restrict
  usage to `atnplex`-owned repos, add `if: github.repository_owner == 'atnplex'`
  guards inside individual workflows.
- **All commits, history, and file contents are publicly visible.** Never commit
  secrets, internal IPs, hostnames, or credentials to this repository.
- **The `.agents/` directory contains agent instructions.** These are operational
  conventions, not secrets, and are safe to be public.

## Secret Management Policy

See [`docs/secret-management.md`](./secret-management.md) for full details.

**Summary:**

- Use `${{ secrets.NAME }}` references only — never hardcode credentials.
- Org-level secrets use the `ATNPLEX_` prefix.
- Consumer repos pass secrets via `secrets: inherit`.
- Rotate any accidentally committed secret immediately, then purge history.

## CODEOWNERS Policy

The `.github/CODEOWNERS` file enforces that **all changes** to this repository
require a review from the `@atnplex/owners` team.

Critical paths with explicit entries:
- `.github/workflows/` — any workflow change requires owner approval
- `templates/` — template changes affect every consumer repo
- `docs/` — documentation must be reviewed for accuracy

This policy is enforced through branch protection rules on `main`.

## Branch Protection Recommendations

See [`docs/branch-protection.md`](./branch-protection.md) for full details.

**Summary:**

- Require 1 owner approval before merging to `main`
- Require status checks to pass
- Restrict direct pushes to `main`
- Require signed commits (recommended)
- Do not allow bypassing these rules

## Required Secrets / Permissions

Enforcing these policies requires **org owner** access to:
- Configure repository settings
- Set branch protection rules
- Manage org secrets

## Notes

- Never commit secrets, tokens, API keys, passwords, or credentials of any kind.
- Run the `detect-secrets` pre-commit hook (see
  `templates/pre-commit-config.yaml`) in all consumer repos to prevent
  accidental secret commits.
- GitHub Advanced Security secret scanning is enabled via `.github/secret_scanning.yml`.
