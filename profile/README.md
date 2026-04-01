# atnplex

Infrastructure automation, self-hosted services, and DevOps tooling.

## Shared CI/CD Workflows

The [`atnplex/.github`](https://github.com/atnplex/.github) repository provides
reusable GitHub Actions workflows for all `atnplex` repositories:

| Workflow | Purpose |
|---|---|
| [`_autofix`](.github/workflows/_autofix.yml) | Auto-format and commit code fixes |
| [`_dependency-review`](.github/workflows/_dependency-review.yml) | Review dependency changes in PRs |
| [`_labeler`](.github/workflows/_labeler.yml) | Auto-label PRs by changed files |
| [`_pr-title-check`](.github/workflows/_pr-title-check.yml) | Enforce Conventional Commits titles |
| [`_release-drafter`](.github/workflows/_release-drafter.yml) | Maintain automated draft releases |
| [`_stale`](.github/workflows/_stale.yml) | Close stale issues and PRs |

See [`docs/workflows.md`](docs/workflows.md) for full documentation.
