# Onboarding

## Purpose

Checklist for `atnplex` org admin users to verify that their access is correctly
configured and that all shared workflows and resources are usable from any
`atnplex` repository.

## Usage

Work through every item in the checklist below. Each admin should complete this
independently to confirm end-to-end access.

## Admin Onboarding Checklist

### Step 1 — Confirm org membership

- [ ] Verify you appear as an **Owner** in
      **github.com/orgs/atnplex/people** with the "Owner" role.
- [ ] Confirm you can see `atnplex/.github` in the org's repository list.

### Step 2 — Confirm access to org secrets

- [ ] Navigate to **github.com/organizations/atnplex/settings/secrets/actions**
- [ ] Confirm that `GITHUB_TOKEN` (built-in) is available.
- [ ] Confirm that `ATNPLEX_ACTIONS_TOKEN` exists (create it if missing;
      see `docs/secret-management.md` for details).
- [ ] Confirm that each secret's **Repository access** is set to
      "All repositories" or covers all relevant `atnplex` repos.

### Step 3 — Confirm self-hosted runner visibility

- [ ] Navigate to **github.com/organizations/atnplex/settings/actions/runners**
- [ ] Confirm at least one `self-hosted`, `linux` runner is listed and online.
- [ ] If no runner is online, pass `runner: ubuntu-latest` as a workflow input
      to temporarily fall back to GitHub-hosted runners (see `docs/runners.md`).

### Step 4 — Confirm `atnplex/.github` Actions access setting

- [ ] Navigate to **github.com/atnplex/.github/settings/actions**
- [ ] Under **Access**, confirm the setting is configured per org policy.
- [ ] Confirm consumer repos can call these reusable workflows.

### Step 5 — Confirm you can trigger workflow dispatches

- [ ] In a test or personal `atnplex` repository, create a caller workflow
      (copy `templates/repo-ci.yml`) and push a commit or open a PR.
- [ ] Confirm all reusable workflow jobs complete successfully:
  - [ ] `autofix.ci`
  - [ ] `Validate PR Title`
  - [ ] `Apply PR Labels`
  - [ ] `Update Draft Release`
  - [ ] `Process Stale Issues and PRs`
  - [ ] `Dependency Review` (public repos only)

### Step 6 — Confirm branch protection and CODEOWNERS

- [ ] Verify that `atnplex/.github` has branch protection enabled on
      `main` (see `docs/branch-protection.md` for recommended settings).
- [ ] Open a test PR to `.github` and confirm that a CODEOWNERS review
      is requested from `@atnplex/owners`.
- [ ] Confirm that the PR cannot be merged without at least 1 owner approval.

### Step 7 — Verify no secrets in the repo

- [ ] Clone `atnplex/.github` locally.
- [ ] Run `git log --all -- '*.env' '*.key' '*.pem'` and confirm no results.
- [ ] Run `detect-secrets scan` (requires `pip install detect-secrets`) and
      confirm no high-confidence findings.

## Required Secrets / Permissions

This checklist requires **org owner** access in GitHub.

## Notes

- Complete this checklist when first joining as an admin and after any
  significant change to workflows, secrets, or runner configuration.
- If any step fails, consult `docs/security.md` and `docs/setup.md` for
  remediation steps.
- All admins should be able to complete every step independently. If one admin
  cannot, investigate and resolve the access gap.
