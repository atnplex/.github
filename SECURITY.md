# Security Policy

We take security seriously and appreciate responsible disclosure of vulnerabilities.

## Supported projects

This policy applies to repositories under the `atnplex` organization unless a repository explicitly defines its own security policy.

## Reporting a vulnerability

If you believe you have found a security issue:

1. **Do not** open a public GitHub issue.
2. Instead, contact us privately via GitHub (security-related contact details may be listed in individual repositories), or by opening a security advisory if the repository supports it.
3. Provide as much detail as possible:
   - affected repository and branch
   - steps to reproduce
   - impact assessment
   - any proof-of-concept code or configuration

We will:

- acknowledge receipt of your report,
- investigate and validate the issue,
- work with you on remediation and coordinated disclosure if appropriate.

## Scope

We are primarily interested in:

- vulnerabilities that could lead to unauthorized access, data exposure, or remote code execution,
- supply-chain issues in our CI/CD or dependency workflows,
- misconfigurations in infrastructure-as-code that may expose sensitive data or control planes.

Low-risk issues (such as minor misconfigurations with no realistic impact) are still welcome, but may be prioritized lower.

## Responsible disclosure

Please give us a reasonable amount of time to investigate and remediate issues before any public disclosure. We do not authorize or endorse any form of active exploitation beyond what is necessary to confirm a vulnerability.
