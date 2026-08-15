# Contributing to the AI Security & QA Engineering Framework

## Repository Structure

- `.claude/` - Framework configuration and internal implementation
  - `agents/` - Specialized agent implementations
  - `skills/` - Domain-specific workflow definitions
  - `rules/` - Execution workflow, tool selection, and severity models
  - `knowledge/` - Reference materials and vulnerability patterns
  - `state/` - Persistent execution state (checkpoints, engagement state)
  - `profiles/` - Behavioral profiles (sanitized for public release)
  - `schemas/` - JSON schemas for state and findings
  - `CLAUDE.md` - Core framework instructions
- Root level - Documentation and project configuration

## How to Contribute

### Documentation Changes

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/documentation-update`
3. Update documentation files in the root directory or appropriate subdirectories
4. Ensure all examples use relative paths (e.g., `.claude/agents/`)
5. Do not expose machine-specific paths (e.g., `C:\Users\...`)
6. Submit a pull request with clear description of changes

### Agent/Rule/Skill Changes

1. **Security First**: All changes must undergo security review
2. **Maintain Separation of Concerns**: 
   - Agents perform specialized work only
   - Rules govern workflow and tool selection
   - Skills define how domain tasks should be performed
3. **Preserve Framework Integrity**:
   - Do not break agent-to-tool mapping contracts
   - Maintain evidence-based validation requirements
   - Keep security/QA separation intact
4. Follow the standard contribution workflow above

### Testing Expectations

- All contributions should maintain backward compatibility where possible
- Update corresponding documentation for any behavioral changes
- Test changes in isolated environment before submission
- For agent/tool changes: verify evidence collection still works correctly

## Pull Request Expectations

1. **Single Responsibility**: PRs should address one logical change
2. **Clear Description**: Explain what, why, and how
3. **Documentation Updates**: Include docs changes when behavior changes
4. **No Secrets**: Verify no credentials, tokens, or sensitive data
5. **Relative Paths Only**: Use `.claude/` relative paths, never machine-specific
6. **Security Review**: All security-related changes require additional scrutiny

## Code Style

- Follow existing patterns in the repository
- Maintain consistent formatting
- Update frontmatter when creating new files in `.claude/`
- Preserve JSON schema compatibility for state/files

## Reporting Issues

Use the standard GitHub issue tracker for:
- Bug reports
- Feature requests
- Documentation improvements
- Security concerns (follow SECURITY.md guidelines)

## Security Notes

- Never commit credentials or sensitive data to the repository
- If you discover a security issue in the framework itself, follow responsible disclosure
- All contributions must respect the framework's authorization-first principle