# Runners

## Purpose

Documents the standard runner expression used across all reusable workflows in
`atnplex/.github`, explains the selection logic, the optional `runner` input
that overrides it, and describes what happens if a self-hosted runner is offline.

## Standard runner expression

All reusable workflows default to this expression when no `runner` input is provided:

```yaml
runs-on: >
  ${{ inputs.runner != '' && inputs.runner
    || (github.event.repository.private == true && github.repository_owner == 'atnplex')
      && fromJSON('[\"self-hosted\",\"linux\"]')
    || 'ubuntu-latest' }}
```

## Overriding the Runner

All workflows accept an optional `runner` input. Pass it from a caller workflow:

```yaml
jobs:
  autofix:
    uses: atnplex/.github/.github/workflows/_autofix.yml@main
    with:
      runner: ubuntu-latest  # force GitHub-hosted runner
    secrets: inherit
```

To use a custom self-hosted runner with multiple labels:

```yaml
with:
  runner: '["self-hosted","linux","arm64"]'
```

## Behavior

| Repository type | Owner | `runner` input | Runner selected |
|---|---|---|---|
| Private | `atnplex` | (omitted) | `self-hosted`, `linux` |
| Public | `atnplex` | (omitted) | `ubuntu-latest` |
| Any | Non-`atnplex` | (omitted) | `ubuntu-latest` |
| Any | Any | `ubuntu-latest` | `ubuntu-latest` |
| Any | Any | `["self-hosted","arm64"]` | `self-hosted`, `arm64` |

## Why this is useful

This keeps private workloads on your own infrastructure while avoiding failures
on public or external repositories that cannot access org-scoped runners. The
`runner` input provides an escape hatch for outages or special cases without
requiring changes to the reusable workflow files themselves.

## What happens if a self-hosted runner is offline

If the self-hosted runner(s) for the `atnplex` org are offline or unavailable:

1. **Queued jobs will wait** up to the GitHub Actions timeout (default 6 hours)
   for a runner to become available.
2. **Jobs will not automatically fall back** to `ubuntu-latest` — the runner
   expression selects based on repository visibility + org ownership, not runner
   availability.
3. **Impact**: all CI for private `atnplex` repositories will stall until a
   runner comes back online.

**Recommended mitigations:**

- Monitor self-hosted runner health via
  **github.com/organizations/atnplex/settings/actions/runners**
- Set up at least 2 self-hosted runners for redundancy
- During an outage, temporarily override by passing `runner: ubuntu-latest`
  from consumer repo caller workflows — no changes to this repo required
- All jobs have `timeout-minutes:` set (10 for lightweight checks, 30 for
  autofix) to prevent indefinite blocking if a runner stalls mid-job

## Required Secrets / Permissions

No secrets required. Viewing runner status requires **org owner** access.

## Notes

- Org self-hosted runners do not automatically work for personal repositories
- Public repositories should prefer GitHub-hosted runners unless there is a
  strong reason otherwise
- If you want self-hosted runners for personal repositories, register runners
  separately for those repositories or accounts
