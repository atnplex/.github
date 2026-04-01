# Security

## Purpose

Consolidates the security policies for `atnplex/.github`: secret management,
CODEOWNERS enforcement, branch protection recommendations, and access control
for the shared workflow infrastructure.

## Access Model

`atnplex/.github` is a **public repository**. Its reusable workflows are
callable by any GitHub repository. Within the `atnplex` organization, private
repos that call these workflows will automatically route to self-hosted runners
via the standard runner expression.

- **Anyone**: can read workflow files (they are public)
- **Org members**: can read and call workflows; can open issues and PRs
- **Org owners**: can read, write, and approve PRs
- **External contributors**: read-only; PRs require owner review

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
- Require status checks to pass (all CI jobs)
- Restrict direct pushes to `main` (owners only, via PR)
- Require signed commits (recommended)
- Do not allow bypassing these rules — even for admins

## Public Repo Considerations

Because this repo is public, workflow files are readable by anyone. This is
intentional — the workflows contain no secrets, no internal hostnames, and no
org-specific logic beyond the runner routing expression (which is low-risk).

If a workflow change introduces anything sensitive, it must be rejected in code
review. The CODEOWNERS rule on `.github/workflows/` ensures all workflow
changes receive owner review.

## Usage

No direct usage — this document is reference material for org admins.

## Required Secrets / Permissions

Enforcing these policies requires **org owner** access to:
- Configure repository access settings
- Set branch protection rules
- Manage org secrets

## Notes

- Run the `detect-secrets` pre-commit hook (see `templates/pre-commit-config.yaml`)
  in all consumer repos to prevent accidental secret commits.
- The `.agents/` directory contains internal agent instructions. These are
  visible publicly; do not add org-sensitive content to those files.
