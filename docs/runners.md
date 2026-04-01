# Runners

## Purpose

Documents the standard runner expression used across all reusable workflows in
`atnplex/.github`, explains the selection logic, describes the optional `runner`
input for overriding runner selection, and documents what happens if a
self-hosted runner is offline.

## Standard runner expression

All workflows use this default expression when no `runner` input is provided:

```yaml
runs-on: >
  ${{
    inputs.runner != ''
      && fromJSON(inputs.runner)
      || ((github.event.repository.private == true && github.repository_owner == 'atnplex')
          && fromJSON('["self-hosted","linux"]')
          || 'ubuntu-latest')
  }}
```

## Overriding the runner via input

Every reusable workflow exposes an optional `runner` input. Pass a plain string
or a JSON array string to force a specific runner:

```yaml
# Force GitHub-hosted runner
with:
  runner: 'ubuntu-latest'

# Force a specific self-hosted runner with labels
with:
  runner: '["self-hosted","linux","arm64"]'
```

Leave `runner` unset (or set to `""`) to use the default expression.

## Behavior

| Repository type | Owner | `runner` input | Runner selected |
|---|---|---|---|
| Private | `atnplex` | (unset) | `self-hosted`, `linux` |
| Public | `atnplex` | (unset) | `ubuntu-latest` |
| Any | Non-`atnplex` | (unset) | `ubuntu-latest` |
| Any | Any | set | value of `runner` input |

## Why this is useful

This keeps private workloads on your own infrastructure while avoiding failures
on public or personal repositories that cannot access org-scoped runners. The
`runner` input escape hatch lets consumer repos override this without modifying
the shared workflow.

## What happens if a self-hosted runner is offline

If the self-hosted runner(s) for the `atnplex` org are offline or unavailable:

1. **Queued jobs will wait** up to the GitHub Actions timeout (default 6 hours)
   for a runner to become available.
2. **Jobs will not automatically fall back** to `ubuntu-latest` — the runner
   expression selects based on the repository being private + org-owned, not
   on runner availability.
3. **Impact**: all CI for private `atnplex` repositories will stall until a
   runner comes back online.

**Recommended mitigations:**

- Monitor self-hosted runner health via
  **github.com/organizations/atnplex/settings/actions/runners**
- Set up at least 2 self-hosted runners for redundancy
- Jobs already have `timeout-minutes:` set so stuck jobs fail fast rather than
  blocking the queue indefinitely
- For non-critical or draft work, pass `runner: 'ubuntu-latest'` in the caller
  to bypass self-hosted runners during an outage

## Required Secrets / Permissions

No secrets required. Viewing runner status requires **org owner** access.

## Notes

- Org self-hosted runners do not automatically work for personal repositories
- Public repositories should generally prefer GitHub-hosted runners unless there
  is a strong reason otherwise
- All jobs have a `timeout-minutes:` set (10 for lightweight checks, 30 for
  autofix) to prevent indefinite blocking if a runner stalls mid-job
