# Security Policy

## Authorized Use Only

This framework is for authorized security assessment, defensive engineering, and approved research only. Do not test systems without explicit permission from the system owner.

## Scope and Authorization

Before active testing, define:

- authorized targets and environments
- in-scope endpoints, repositories, hosts, and APIs
- excluded systems and third-party services
- authorized test accounts and data
- allowed operations and safety limits

Discovery does not create authorization. A newly discovered asset remains untested until its scope is confirmed.

## Safe Testing

Use controlled, non-destructive, rate-limited testing with synthetic data and test accounts where possible. Preserve evidence without copying unnecessary sensitive information.

For production environments, default to low-impact, monitoring-aware validation. Do not perform denial-of-service testing, data deletion, account lockout, high-volume scanning, uncontrolled exploitation, or other destructive actions unless explicitly authorized and appropriate.

If scope becomes unclear, authorization fails, sensitive data appears unexpectedly, or service stability is at risk: stop active testing and document the limitation.

## Secrets and Private Data

Never commit or publicly post:

- API keys, tokens, passwords, credentials, or private keys
- real target URLs or infrastructure details from private engagements
- customer or personal data
- private findings or unredacted evidence
- local MCP configuration containing secrets
- machine-specific paths or local debug artifacts

Use placeholders in examples. Rotate exposed credentials immediately through the appropriate owner.

## Responsible Disclosure

For vulnerabilities in this framework:

1. Do not open a public issue containing exploitable details.
2. Contact the project maintainers through the repository's private security channel, when available.
3. Include affected version or commit, impact, reproducible steps, and a minimal sanitized proof.
4. Do not include secrets, personal data, or unrelated target information.
5. Allow maintainers reasonable time to assess and remediate before public disclosure.

If no private channel is configured, open a minimal issue asking for a private reporting channel without including sensitive details.

## Reporting Security Issues

Report framework security issues through the repository's configured GitHub security contact or private maintainer channel. Include only information needed to reproduce and triage the issue. Never publish credentials or live exploitation data in an issue, pull request, discussion, or README.

For security assessments performed with this framework, follow the engagement's reporting procedure rather than publishing results in this repository.

## Security Design Principles

- Authorization and scope precede active testing.
- Tool output is evidence, not an automatic confirmed vulnerability.
- Findings require validation, impact analysis, and confidence assessment.
- Security findings and functional QA failures remain distinct.
- Tool failures and coverage limitations must be reported honestly.
