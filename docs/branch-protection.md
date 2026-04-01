# Branch Protection

## Purpose

Documents the recommended branch protection rules for the `main` branch of every
repository in the `atnplex` organization, including `atnplex/.github` itself.
These rules enforce code review, CI gating, and commit hygiene.

## Recommended Rules for `main`

Configure these rules under **Settings → Branches → Branch protection rules**
for the `main` branch.

### 1. Require pull request reviews before merging

- **Required approving reviews**: 1 (minimum; increase for critical repos)
- **Dismiss stale reviews when new commits are pushed**: ✅ enabled
- **Require review from Code Owners**: ✅ enabled (enforces `CODEOWNERS`)
- **Restrict who can dismiss pull request reviews**: org owners only

### 2. Require status checks to pass before merging

- **Require branches to be up to date before merging**: ✅ enabled
- **Required status checks** (add all CI jobs from `.github/workflows/ci.yml`):
  - `autofix.ci`
  - `Validate PR Title`
  - `Dependency Review` (for public repos)

### 3. Restrict direct pushes to `main`

- **Restrict pushes**: ✅ enabled
- **Who can push**: org owners only (`@atnplex/owners` team)
- Direct pushes to `main` from feature branches are not permitted; all changes
  must go through a pull request.

### 4. Require signed commits (recommended)

- **Require signed commits**: ✅ strongly recommended
- All commits merged to `main` should be GPG-signed.

### 5. Do not allow bypassing the above rules

- **Do not allow bypassing the above settings**: ✅ enabled
- Even repository admins and org owners must go through the PR process.

### 6. Additional hardening (optional but recommended)

- **Require linear history**: ✅ recommended
- **Allow force pushes**: ❌ disabled for `main`
- **Allow deletions**: ❌ disabled for `main`

## Notes

- These rules must be configured manually via the GitHub UI or the GitHub API /
  Terraform. They cannot be enforced from this repository's files alone.
- The `CODEOWNERS` file (`.github/CODEOWNERS`) works in conjunction with branch
  protection rule #1 to ensure that any change to a critical path requires an
  explicit owner review.

## Usage

```bash
# Example: configure branch protection via GitHub CLI
gh api repos/atnplex/.github/branches/main/protection \
  --method PUT \
  --field required_status_checks='{"strict":true,"contexts":[]}' \
  --field enforce_admins=true \
  --field required_pull_request_reviews='{"required_approving_review_count":1,"dismiss_stale_reviews":true,"require_code_owner_reviews":true}' \
  --field restrictions=null
```

## Required Secrets / Permissions

No secrets are required to document or apply these rules. A GitHub token with
`repo` scope is required to configure branch protection via the API.

## Related Files

- `.github/CODEOWNERS` — defines who owns which paths
- `docs/onboarding.md` — checklist for admins to verify their access
- `docs/security.md` — overall security policy
