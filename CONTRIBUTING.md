# Contributing

Thank you for helping improve the AI Security & QA Engineering Framework. Contributions should improve evidence quality, safety, reproducibility, or usability without weakening authorization boundaries.

## Repository Structure

- `.claude/CLAUDE.md` — framework-wide principles and architecture
- `.claude/agents/` — specialized agent responsibilities
- `.claude/skills/` — reusable domain workflows
- `.claude/rules/` — workflow, tool-selection, and severity policy
- `.claude/knowledge/` — security and testing reference material
- `.claude/state/` — public state guidance and sanitized template state
- `.claude/schemas/` — machine-readable state schemas
- `.claude/profiles/` — behavioral and safety profiles
- Root files — public documentation and project metadata

## Before Contributing

1. Read `README.md`, `SECURITY.md`, and relevant `.claude/` guidance.
2. Confirm your change does not include credentials, tokens, personal data, private engagement data, logs, or machine-specific configuration.
3. Use relative project paths in documentation and examples.
4. Preserve the separation between orchestration, specialized agents, tools, validation, and reporting.
5. Do not claim capabilities or tool integrations that are not verified.

## Documentation Changes

- Keep documentation accurate and concise.
- Document the actual integration type: MCP capability, CLI, or CLI/TUI.
- Do not describe unverified headless automation as supported.
- Update related documentation when behavior or public contracts change.
- Use sanitized examples and placeholders.

## Agent, Rule, Skill, Knowledge, and Schema Changes

- Keep agent responsibilities within their defined domain.
- Keep workflow order in rules, tool procedures in tool-selection policy, and risk classification in the severity model.
- Preserve security and QA separation.
- Preserve state and schema compatibility unless a deliberate migration is documented.
- Route security-relevant findings through validation and evidence correlation.
- Do not add private system prompts, credentials, or engagement artifacts.

## Testing Expectations

Before opening a pull request:

- Validate changed Markdown and JSON syntax.
- Check referenced paths exist.
- Run relevant tests or tool checks available in your environment.
- Confirm failures and unavailable tools are documented rather than hidden.
- For state changes, validate the template against `.claude/schemas/engagement-state.schema.json`.
- For security changes, include safe, reproducible validation steps.

Do not run active security tests against systems without explicit authorization and scope.

## Pull Requests

- Create one focused branch and pull request per logical change.
- Explain what changed, why, validation performed, and known limitations.
- Keep diffs focused; avoid unrelated reformatting or architecture redesign.
- Do not commit `.env` files, local settings, logs, generated reports, screenshots, credentials, or private findings.
- Identify any changes that affect public behavior, tool contracts, schemas, or recovery behavior.

## Reporting Problems

Use GitHub Issues for public bugs, documentation problems, and feature requests that contain no sensitive details. For security concerns, follow `SECURITY.md` and use a private reporting channel.
