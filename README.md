# AI Security & QA Engineering Framework

A coordinated engineering system for systematic software analysis, security assessment, and quality assurance through AI-powered orchestrated workflows.

## Overview

This framework transforms Claude Code into an AI Security & QA Engineering Team capable of coordinating:

- **Static Analysis** - Source-code vulnerability detection via Semgrep
- **Dynamic Security Testing** - Runtime security assessment via pentest-ai / PentesterFlow  
- **Functional QA Testing** - End-to-end validation via TestSprite
- **Vulnerability Analysis** - Evidence-based finding correlation and classification
- **Risk Assessment** - Business-context severity scoring
- **Professional Reporting** - Evidence-backed security reports

## Architecture

```
Claude Code
    |
    v
Security Orchestrator
    |
    +-- Code Review Agent --> Semgrep --> Findings --> Vulnerability Analyst
    |
    +-- Security Agent --> pentest-ai/PentesterFlow --> Security Evidence --> Vulnerability Analyst
    |
    +-- QA Agent --> TestSprite --> Test Results --> Vulnerability Analyst
    |
    v
Vulnerability Analyst --> Severity Classification --> Report Generator
```

## Agents

| Agent | Responsibility | Primary Tool |
|-------|---------------|--------------|
| Security Orchestrator | Assessment coordination, tool selection, execution flow | Framework internal |
| Security Agent | Dynamic security testing, reconnaissance, runtime validation | pentest-ai, PentesterFlow |
| Code Review Agent | Static security analysis, safe coding review, pattern detection | Semgrep |
| QA Agent | Functional testing, workflow validation, regression testing | TestSprite |
| Vulnerability Analyst | Finding correlation, validation, severity classification | Analyst reasoning |
| Report Generator | Professional reporting, remediation guidance | Report generation |

## Skills

- `security-audit` - Structured penetration testing workflows
- `code-review` - Source-code security analysis  
- `qa-testing` - Functional testing and validation

## Rules

- `workflow.md` - Execution orchestration and phase sequencing
- `tool-selection.md` - Evidence-based tool selection
- `severity-model.md` - Risk classification framework

## Knowledge

- `security-patterns.md` - Common vulnerability patterns
- `common-vulnerabilities.md` - Vulnerability class guidance
- `testing-strategy.md` - Testing approach recommendations

## State / Checkpoint / Resume

Framework supports persistent execution state for:
- Workflow pause/resume
- Task recovery
- Checkpoint-based continuation
- State persists under `.claude/state/`

## Tool Integrations

| Tool | Category | Integration Type | Installation |
|------|----------|------------------|--------------|
| Semgrep | Static Analysis | CLI/TUI | `pip install semgrep` or Python installation |
| pentest-ai | Dynamic Security | MCP capability | Claude Code MCP configuration |
| PentesterFlow | Penetration Testing | CLI/TUI | Standalone executable |
| TestSprite | QA Testing | MCP capability | Claude Code MCP configuration |

**Important**: Tool availability depends on your environment and MCP configuration. Always verify tool installation before execution.

## Prerequisites

1. **Claude Code** installed and authenticated
2. **Required MCP servers** for pentest-ai and TestSprite (if using)
3. **Semgrep** installed for static analysis (`semgrep` CLI or Python module)
4. **PentesterFlow** installed for extended penetration testing workflows

## Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/[your-org]/ai-security-qa-framework.git
   cd ai-security-qa-framework
   ```

2. Configure Claude Code:
   - Copy `.claude/settings.json` if present
   - Configure MCP servers in Claude settings
   - Verify tool paths match your environment

3. Install tools:
   ```bash
   # Semgrep (static analysis)
   pip install semgrep
   
   # Or use standalone executable
   semgrep --version
   
   # PentesterFlow (pen testing)
   # Download from official release page
   pentesterflow --help
   ```

## First Run

Execute a simple assessment:
```bash
# Run framework assessment
cd .claude
# Framework will orchestrate tools based on scope
```

## State Management

The framework supports:
- **Resume capability**: Continue interrupted assessments from last checkpoint
- **Task persistence**: Maintain progress across sessions
- **Checkpoint tracking**: Atomic workflow state preservation
- **Recovery handling**: Automatic state validation on resume

State files live in `.claude/state/` and are safe to commit (empty templates only).

## Limitations

- Tool availability varies by environment
- Dynamic testing requires authorized targets
- Production testing must be scoped and non-destructive
- MCP servers require proper configuration for runtime tools

## Security & Authorization

- **Always verify authorization** before security testing
- **Scope must be explicit** - discovered endpoints do not expand scope
- **Production safety** - use low-impact testing methods
- **Credential safety** - never expose secrets in reports

See [SECURITY.md](SECURITY.md) for complete security guidelines.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines.

## License

MIT License - see [LICENSE](LICENSE) for details.

## Version

v0.1.0 Preview