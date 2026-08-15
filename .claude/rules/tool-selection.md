# Tool Selection Policy

## 1. Purpose

This document defines the authoritative tool-selection policy
for the AI Security & QA Engineering Framework.

Its purpose is to ensure that every task is assigned to:

    The Right Agent
        +
    The Right Tool
        +
    The Right Testing Method
        +
    The Right Stage


The framework must not select tools based only on:

    Tool Availability
    Tool Popularity
    Tool Name
    Previous Usage
    Convenience


Tool selection must be driven by:

    Objective
    Scope
    Application Layer
    Evidence Required
    Risk
    Environment
    Authorization
    Tool Capability


# 2. Authority

This file is the authoritative policy for tool selection.

The Security Orchestrator must follow this policy.

Agents must not override it without a documented reason.

The relationship is:

    CLAUDE.md
        |
        v
    Security Orchestrator
        |
        v
    Tool Selection Policy
        |
        +-----------------------------+
        |             |               |
        v             v               v
    Security       Static           QA
    Tools          Analysis         Tools
        |             |               |
        v             v               v
    Findings       Findings        Results
        |             |               |
        +-------------+---------------+
                      |
                      v
             Vulnerability Analyst
                      |
                      v
               Report Generator


# 3. Framework Tools

The current framework may use:

    Semgrep
    pentest-ai
    PentesterFlow
    TestSprite


These tools have different responsibilities.

They must not be treated as interchangeable.


# 4. Primary Tool Classification

## Static Analysis

Primary:

    Semgrep


Purpose:

    Source-Code Analysis
    Pattern Detection
    Data Flow Analysis
    Security Pattern Detection
    Vulnerability Candidate Discovery


## Dynamic Security Testing

Primary:

    pentest-ai
    PentesterFlow


Purpose:

    Runtime Security Testing
    Reconnaissance
    Vulnerability Discovery
    API Security Testing
    Web Security Testing
    Attack-Path Validation
    Dynamic Assessment


## Functional / QA Testing

Primary:

    TestSprite


Purpose:

    Functional Testing
    API Testing
    Integration Testing
    End-to-End Testing
    Regression Testing
    Workflow Validation


# 5. Agent-to-Tool Mapping

The framework should generally follow:

    Security Orchestrator
        |
        +--> Security Agent
        |       |
        |       +--> pentest-ai
        |       +--> PentesterFlow
        |
        +--> Code Review Agent
        |       |
        |       +--> Semgrep
        |
        +--> QA Agent
        |       |
        |       +--> TestSprite
        |
        +--> Vulnerability Analyst
        |       |
        |       +--> Correlation
        |       +--> Validation
        |
        +--> Report Generator
                |
                +--> Reporting


The Orchestrator decides:

    Which Agent should act.


The Agent decides:

    Which supported tool should be used.


This separation prevents uncontrolled tool usage.


# 6. Core Selection Rule

Before selecting a tool, answer:

    What am I trying to prove?


Possible objectives:

    Source-Level Weakness
    Runtime Vulnerability
    Functional Defect
    API Behavior
    End-to-End Behavior
    Exploitability
    Regression
    Root Cause


Then select the tool that produces
the strongest evidence for that objective.


# 7. Decision Matrix

Use this general mapping:

| Objective | Primary Tool | Agent |
|-----------|--------------|-------|
| Source-code security analysis | Semgrep | Code Review Agent |
| Security pattern detection | Semgrep | Code Review Agent |
| Data-flow security analysis | Semgrep | Code Review Agent |
| Web security testing | pentest-ai / PentesterFlow | Security Agent |
| API security testing | pentest-ai / PentesterFlow | Security Agent |
| Reconnaissance | pentest-ai / PentesterFlow | Security Agent |
| Dynamic vulnerability discovery | pentest-ai / PentesterFlow | Security Agent |
| Functional frontend testing | TestSprite | QA Agent |
| Backend functional testing | TestSprite | QA Agent |
| E2E testing | TestSprite | QA Agent |
| Regression testing | TestSprite | QA Agent |
| Finding correlation | Analyst reasoning | Vulnerability Analyst |
| Finding validation | Analyst reasoning + evidence | Vulnerability Analyst |
| Professional reporting | Report Generator | Report Generator |


# 8. Never Use a Tool Without a Task

Do not run a tool simply because:

    It is installed.

Do not run:

    Semgrep

just because source code exists.

Do not run:

    TestSprite

just because an application has a frontend.

Do not run:

    pentest-ai

just because a target URL exists.


There must be a defined objective.


# 9. Security Assessment Selection

For a security assessment:

    Security Orchestrator
        |
        v
    Determine Scope
        |
        v
    Identify Application Layers
        |
        +------------------+
        |                  |
        v                  v
    Source Available    Runtime Available
        |                  |
        v                  v
     Semgrep         Dynamic Security
                         |
                    +----+----+
                    |         |
                    v         v
                pentest-ai PentesterFlow


If functional behavior is also in scope:

    Add TestSprite


# 10. Source Code Available

If source code is available:

    Prefer static analysis where relevant.


Typical flow:

    Repository
        |
        v
    Code Review Agent
        |
        v
    Semgrep
        |
        v
    Candidate Findings
        |
        v
    Vulnerability Analyst


Static analysis should establish:

    Potential Root Cause


It should not automatically establish:

    Exploitability


# 11. Source Code Unavailable

If source code is unavailable:

    Do not attempt source-code analysis.


Use:

    Dynamic Security Testing
    API Testing
    Functional Testing


depending on scope.


# 12. Runtime Target Available

If a reachable runtime target exists:

    Dynamic testing may be appropriate.


Possible tools:

    pentest-ai
    PentesterFlow


Selection depends on:

    Capability
    Target Type
    Required Evidence
    Engagement Scope


# 13. Web Application

For web applications:

    Recon
        |
        v
    Application Mapping
        |
        v
    Dynamic Security Testing
        |
        v
    Functional / E2E Testing
        |
        v
    Correlation


Security testing:

    pentest-ai / PentesterFlow


Functional testing:

    TestSprite


Static source analysis when available:

    Semgrep


# 14. API

For API-focused engagements:

    API Discovery
        |
        v
    API Functional Testing
        |
        v
    API Security Testing
        |
        v
    Validation


Functional:

    TestSprite


Security:

    pentest-ai / PentesterFlow


Static:

    Semgrep


# 15. Frontend

For frontend quality:

    TestSprite


For frontend security source analysis:

    Semgrep


For runtime web security:

    pentest-ai / PentesterFlow


Do not confuse:

    Frontend Testing

with:

    Frontend Security Testing


# 16. Backend

For backend source analysis:

    Semgrep


For backend functional behavior:

    TestSprite


For backend runtime security:

    pentest-ai / PentesterFlow


The same backend may therefore require
multiple tools for different objectives.


# 17. Authentication

Authentication should normally involve:

    QA Testing
        +
    Security Testing


Functional authentication:

    TestSprite


Security authentication:

    pentest-ai / PentesterFlow


Source implementation:

    Semgrep


Example:

    Login works correctly
        ->
    TestSprite


    Authentication bypass
        ->
    Dynamic Security Tool


    Missing authentication check in code
        ->
    Semgrep


# 18. Authorization

Authorization is especially important.

Use layered validation:

    Source Analysis
        +
    Functional Testing
        +
    Dynamic Security Testing


Possible mapping:

    Missing Authorization Check
        ->
    Semgrep


    Unauthorized Workflow
        ->
    TestSprite


    Exploitable Access-Control Bypass
        ->
    pentest-ai / PentesterFlow


Then:

    Vulnerability Analyst


correlates the evidence.


# 19. Business Logic

Business logic vulnerabilities often require:

    Functional Understanding
        +
    Runtime Testing


Start with:

    TestSprite


when the issue is workflow behavior.


Use:

    pentest-ai / PentesterFlow


when the objective is security exploitation.


If source code is available:

    Semgrep


may help identify root cause.


# 20. Sensitive Data Exposure

Use:

    Static Analysis
        +
    Dynamic Testing
        +
    Functional Validation


Possible mapping:

    Semgrep
        ->
    Sensitive Data Handling


    pentest-ai / PentesterFlow
        ->
    Runtime Exposure


    TestSprite
        ->
    Expected User-Level Behavior


# 21. File Upload

File upload should be evaluated in layers:

    Functional Upload
        ->
    TestSprite


    Security Validation
        ->
    pentest-ai / PentesterFlow


    Upload Handling Code
        ->
    Semgrep


Do not treat successful upload
as proof of secure upload handling.


# 22. Authentication State

For session-related behavior:

    Functional:
        TestSprite


    Runtime Security:
        pentest-ai / PentesterFlow


    Source:
        Semgrep


This may reveal:

    Session Handling
    Token Validation
    Logout Behavior
    Authorization State


# 23. Error Handling

For functional error behavior:

    TestSprite


For security-sensitive error disclosure:

    pentest-ai / PentesterFlow


For unsafe error-handling code:

    Semgrep


Examples:

    Incorrect HTTP status
        ->
    QA


    Sensitive stack trace exposure
        ->
    Dynamic Security


    Unsafe exception handling
        ->
    Static Analysis


# 24. Dependency / Configuration Issues

If the issue is source configuration:

    Semgrep


If it is runtime configuration:

    Dynamic Security Testing


If it affects application behavior:

    TestSprite


Tool selection depends on:

    What must be proven.


# 25. Tool Combination

Multiple tools should be used when
they provide complementary evidence.

Example:

    Semgrep
        |
        v
    Candidate Authorization Weakness
        |
        v
    TestSprite
        |
        v
    Unauthorized Workflow Observed
        |
        v
    pentest-ai
        |
        v
    Exploitation Confirmed
        |
        v
    Vulnerability Analyst


This is preferred over:

    Running every tool blindly.


# 26. Evidence Strength

Tool selection should consider
the type of evidence produced.

Generally:

    Static Candidate
        <
    Runtime Observation
        <
    Reproducible Exploitation


But this is not an absolute ranking.

Evidence quality depends on:

    Context
    Reproducibility
    Reliability
    Scope
    Validation


# 27. Static Analysis Limitation

Static analysis can identify:

    Potential Vulnerabilities
    Dangerous Patterns
    Data Flows
    Missing Controls


It may not prove:

    Runtime Exploitability


Therefore:

    Static Finding
        |
        v
    Candidate
        |
        v
    Validation


# 28. Dynamic Testing Limitation

Dynamic testing can prove:

    Runtime Behavior
    Exploitability
    Access Control Failure
    Input Validation Failure


It may not fully explain:

    Root Cause


Therefore:

    Dynamic Finding
        |
        v
    Correlation
        |
        v
    Source Analysis when available


# 29. QA Testing Limitation

QA testing can prove:

    Functional Behavior
    Workflow Failure
    Regression
    User-Level Behavior


It does not automatically prove:

    Security Vulnerability


Security-relevant failures must be passed to:

    Vulnerability Analyst


# 30. TestSprite Limitation

TestSprite is a:

    QA / Testing Tool


It should not be used as:

    Final Vulnerability Validator


Its results are evidence.

Security conclusions require:

    Security Analysis
    Validation
    Correlation


# 31. pentest-ai Limitation

pentest-ai should be used within:

    Authorized Security Scope


Its output is:

    Security Evidence


not automatically:

    Final Confirmed Findings


Results must pass through:

    Vulnerability Analyst


# 32. PentesterFlow Limitation

PentesterFlow should be treated as:

    Dynamic Security Capability


Its output requires:

    Evidence Review
    Validation
    Correlation


Do not report automated output blindly.


## PentesterFlow Integration Contract

PentesterFlow is a standalone CLI/TUI capability used by the Security Agent for broader penetration-testing workflows, multi-stage testing, attack-chain analysis, and deeper adversarial validation.

Verified installation:
- Executable: `<verified-local-pentesterflow-cli-path>`
- Version: 0.1.20
- Interface: CLI/TUI
- Not an MCP server.

Selection rule:
Use PentesterFlow when its broader workflow capabilities provide meaningful additional coverage beyond the initial dynamic assessment.

Do not select PentesterFlow merely because it is available.

Execution details, configuration flags, failure handling, and session behavior are defined by:

.claude/agents/security-agent.md

Do not duplicate the full invocation contract from security-agent.md here.


# 33. Semgrep Limitation

Semgrep results must be interpreted
within application context.

Avoid:

    "Semgrep reported it, therefore it is a vulnerability."


Instead:

    Semgrep Candidate
        |
        v
    Context Analysis
        |
        v
    Validation
        |
        v
    Final Finding


## Semgrep Integration Contract

Semgrep is a standalone CLI capability owned by the Code Review Agent. It is not an MCP server.

Verified installation:
- Executable: `<verified-local-semgrep-cli-path>`
- Version: 1.172.0

PATH limitation:
The `semgrep` command is NOT currently available in PATH. Do not assume `semgrep` resolves from PATH.

Selection rule:
Use Semgrep when source code is available and static security analysis is relevant.

Do not select Semgrep merely because it is installed.

Semgrep output is candidate evidence, not a final vulnerability conclusion.

Failures and coverage limitations must be recorded.

Execution details are defined by:

.claude/agents/code-review-agent.md

Do not duplicate the full Semgrep invocation/evidence contract from code-review-agent.md here.


# 34. Tool Availability Failure

If a preferred tool is unavailable:

    Do not fabricate results.


Instead:

    Record Tool Failure


Then determine whether:

    Alternative Authorized Tool


can provide equivalent evidence.


# 35. Tool Failure Hierarchy

If a tool fails:

    1. Diagnose
    2. Retry when appropriate
    3. Check scope / configuration
    4. Use an approved alternative
    5. Document limitation


Never silently skip the intended test.


# 36. No Evidence Fabrication

Never claim:

    TestSprite passed tests

unless it actually executed them.


Never claim:

    pentest-ai found no vulnerabilities

unless the relevant scope was actually tested.


Never claim:

    Semgrep found no issues

unless the intended analysis actually ran.


Never claim:

    PentesterFlow confirmed exploitation

unless evidence exists.


# 37. Scope Before Tool

Always determine:

    Scope


before:

    Tool Selection


The order is:

    Scope
        |
        v
    Objective
        |
        v
    Target
        |
        v
    Evidence Required
        |
        v
    Tool


Never reverse this process.


# 38. Authorization Before Execution

Before dynamic testing verify:

    Target Is Authorized
    Testing Is In Scope
    Environment Is Known
    Credentials Are Appropriate
    Safety Constraints Are Known


Do not expand scope because
a tool discovers additional targets.


# 39. Production Safety

For production:

    Prefer Low-Impact Testing


Avoid:

    Destructive Tests
    High-Volume Scanning
    Account Lockout
    Data Deletion
    Resource Exhaustion


unless explicitly authorized.


# 40. Tool Selection by Environment

Development:

    Static Analysis
    Functional Testing
    Controlled Dynamic Testing


Staging:

    Full QA
    Security Testing
    E2E
    Regression


Production:

    Carefully Scoped Runtime Testing
    Read-Only Validation
    Monitoring-Aware Testing


The environment changes the acceptable tool strategy.


# 41. Tool Selection by Evidence

If you need:

    Source Location

use:

    Semgrep


If you need:

    Runtime Exploitability

use:

    pentest-ai / PentesterFlow


If you need:

    User Workflow Behavior

use:

    TestSprite


If you need:

    Correlated Security Conclusion

use:

    Vulnerability Analyst


# 42. Tool Selection by Layer

| Layer | Functional | Static Security | Dynamic Security |
|-------|------------|-----------------|------------------|
| Frontend | TestSprite | Semgrep | pentest-ai / PentesterFlow |
| Backend | TestSprite | Semgrep | pentest-ai / PentesterFlow |
| API | TestSprite | Semgrep | pentest-ai / PentesterFlow |
| Auth | TestSprite | Semgrep | pentest-ai / PentesterFlow |
| Authorization | TestSprite | Semgrep | pentest-ai / PentesterFlow |
| Business Logic | TestSprite | Semgrep | pentest-ai / PentesterFlow |
| Integration | TestSprite | Semgrep | pentest-ai / PentesterFlow |


# 43. Minimal Tool Principle

Use the minimum number of tools
required to obtain sufficient evidence.

Example:

    Simple UI Regression

requires:

    TestSprite


Do not run:

    Semgrep
    pentest-ai
    PentesterFlow


without a reason.


# 44. Maximum Evidence Principle

For high-risk findings,
multiple complementary tools may be justified.

Example:

    High-Risk Authorization Issue

may use:

    Semgrep
    +
    TestSprite
    +
    pentest-ai


when all are available and in scope.


# 45. Avoid Redundant Execution

Do not repeatedly run tools that provide
the same evidence.

Example:

    Three identical dynamic scanners

may provide little additional value.


Prefer:

    Complementary Evidence


# 46. Parallel Execution

Independent tasks may execute in parallel.

Example:

    Semgrep
        +
    TestSprite


can run independently when:

    Scope
    Environment
    Dependencies


allow it.


Do not parallelize when:

    One test depends on another result.


# 47. Sequential Execution

Use sequential execution when
the next stage requires previous evidence.

Example:

    Semgrep
        |
        v
    Candidate Finding
        |
        v
    Dynamic Validation
        |
        v
    Vulnerability Analyst


# 48. Escalation Strategy

Use progressive escalation:

    Level 1:
        Basic Analysis


    Level 2:
        Specialized Tool


    Level 3:
        Cross-Tool Validation


    Level 4:
        Analyst Validation


Do not begin with maximum complexity
when simpler evidence is sufficient.


# 49. Finding Escalation

A candidate finding may progress:

    Tool Alert
        |
        v
    Agent Review
        |
        v
    Reproduction
        |
        v
    Cross-Tool Correlation
        |
        v
    Vulnerability Analyst
        |
        v
    Confirmed Finding


Not every candidate must reach every level.


# 50. Tool Selection for Vulnerability Classes

General guidance:

| Vulnerability Class | Static | Dynamic | QA |
|---------------------|--------|---------|-----|
| Injection | Semgrep | pentest-ai / PentesterFlow | TestSprite |
| XSS | Semgrep | pentest-ai / PentesterFlow | TestSprite |
| Broken Access Control | Semgrep | pentest-ai / PentesterFlow | TestSprite |
| Authentication Bypass | Semgrep | pentest-ai / PentesterFlow | TestSprite |
| IDOR / BOLA | Semgrep | pentest-ai / PentesterFlow | TestSprite |
| Sensitive Data Exposure | Semgrep | pentest-ai / PentesterFlow | TestSprite |
| Business Logic | Semgrep | pentest-ai / PentesterFlow | TestSprite |
| Security Misconfiguration | Semgrep | pentest-ai / PentesterFlow | TestSprite |
| Dependency Risk | Semgrep / project tooling | Runtime validation | TestSprite when relevant |
| Functional Defect | Not primary | Not primary | TestSprite |


This table is guidance, not an automatic execution command.


# 51. API Authorization Example

Scenario:

    GET /api/orders/{id}


Question:

    Can User A access User B's order?


Use:

    TestSprite
        ->
    Validate expected application behavior


Then:

    pentest-ai / PentesterFlow
        ->
    Test unauthorized access


If source code is available:

    Semgrep
        ->
    Locate authorization logic


Then:

    Vulnerability Analyst
        ->
    Correlate


# 52. SQL Injection Example

Scenario:

    Search Endpoint


Static:

    Semgrep
        ->
    Identify unsafe query construction


Dynamic:

    pentest-ai / PentesterFlow
        ->
    Test exploitability


QA:

    TestSprite
        ->
    Verify normal search behavior


Analyst:

    Vulnerability Analyst
        ->
    Validate finding


# 53. XSS Example

Static:

    Semgrep
        ->
    Dangerous output handling


Dynamic:

    pentest-ai / PentesterFlow
        ->
    Runtime validation


QA:

    TestSprite
        ->
    Verify expected rendering / workflow


Analyst:

    Correlate evidence


# 54. Authentication Example

Functional:

    TestSprite


Security:

    pentest-ai / PentesterFlow


Source:

    Semgrep


Correlation:

    Vulnerability Analyst


Reporting:

    Report Generator


# 55. Regression Example

After fixing a vulnerability:

    Original Security Test
        |
        v
    TestSprite Regression
        |
        v
    Dynamic Security Retest
        |
        v
    Vulnerability Analyst
        |
        v
    Report Generator


This verifies:

    Security Fix
        +
    Functional Stability


# 56. Tool Selection Output

When the Orchestrator selects tools,
the internal decision should conceptually contain:

    Objective
    Target
    Scope
    Required Evidence
    Selected Agent
    Selected Tool
    Reason
    Dependencies
    Safety Constraints
    Expected Output


Example:

    Objective:
        Validate authorization bypass

    Target:
        REST API

    Agent:
        Security Agent

    Tool:
        pentest-ai

    Reason:
        Runtime authorization validation

    Supporting Tool:
        TestSprite

    Reason:
        Functional workflow verification


# 57. Tool Selection Must Be Explainable

The framework should always be able to answer:

    Why was this tool selected?


Valid answer:

    "TestSprite was selected because the objective
     is end-to-end functional validation."


Invalid answer:

    "Because it was available."


# 58. Tool Selection Must Be Reproducible

Given the same:

    Scope
    Objective
    Environment
    Tool Availability


the framework should generally produce
a similar tool-selection decision.


Exceptions should be documented.


# 59. Tool Selection and Knowledge

Knowledge files may help identify:

    Vulnerability Type
    Security Pattern
    Testing Strategy


Relevant knowledge:

    .claude/knowledge/security-patterns.md
    .claude/knowledge/common-vulnerabilities.md
    .claude/knowledge/testing-strategy.md


Knowledge informs:

    Selection


but does not override:

    Scope
    Authorization
    Evidence Requirements


# 60. Tool Selection and Workflow

The workflow defined in:

    .claude/rules/workflow.md


controls:

    When a tool should execute.


This file controls:

    Which tool is appropriate.


The distinction is:

    workflow.md
        ->
    WHEN


    tool-selection.md
        ->
    WHICH


# 61. Tool Selection and Severity

Severity defined in:

    .claude/rules/severity-model.md


must not determine whether
a vulnerability exists.


Severity may influence:

    Validation Depth
    Evidence Requirements
    Retesting Priority


but does not replace validation.


# 62. Tool Selection and Reporting

Reporting must only consume
validated results.

The reporting layer is:

    .claude/skills/reporting/SKILL.md


The Report Generator should know:

    Which tool produced the evidence


but should not treat tool identity
as proof of severity or validity.


# 63. Tool Selection and QA

QA selection is governed by:

    .claude/skills/qa-testing/SKILL.md


TestSprite should be selected for:

    Functional
    E2E
    Integration
    Regression


when appropriate.


# 64. Tool Selection and Code Review

Static security analysis is governed by:

    .claude/skills/code-review/SKILL.md


Semgrep should be selected when
source-level analysis can provide useful evidence.


# 65. Tool Selection and Security Audit

Dynamic security assessment is governed by:

    .claude/skills/security-audit/SKILL.md


Use:

    pentest-ai
    PentesterFlow


according to their capabilities and the engagement scope.


# 66. Failure Recovery

If selected tool fails:

    Do not silently switch tools.


Instead:

    Record Failure
        |
        v
    Determine Cause
        |
        v
    Evaluate Alternative
        |
        v
    Continue or Document Limitation


Alternative selection must still follow this policy.


# 67. Alternative Tool Selection

An alternative tool is acceptable only when
it provides sufficiently relevant evidence.

Example:

    Preferred:
        TestSprite


If unavailable:

    Approved QA alternative


may be used.


Do not replace:

    Functional Testing

with:

    Static Analysis


just because Semgrep is available.


# 68. No Forced Tool Usage

The framework does not require every engagement
to use every tool.

A valid assessment may use:

    Semgrep only


or:

    TestSprite only


or:

    pentest-ai + TestSprite


or:

    Semgrep + pentest-ai + TestSprite


depending on:

    Scope
    Objective
    Evidence
    Risk


# 69. Full-Stack Assessment

For a full-stack application,
a comprehensive assessment may use:

    Semgrep
        +
    pentest-ai / PentesterFlow
        +
    TestSprite


Typical flow:

    Source Analysis
        |
        v
    Runtime Security
        |
        v
    Functional / E2E
        |
        v
    Correlation
        |
        v
    Validation
        |
        v
    Reporting


# 70. Tool Selection Anti-Patterns

Never:

    Run every tool automatically.


Never:

    Trust tool severity blindly.


Never:

    Treat tool count as assessment quality.


Never:

    Treat scan count as coverage.


Never:

    Replace validation with automation.


Never:

    Expand scope because a tool discovers
    additional targets.


Never:

    Generate findings solely from tool output.


# 71. Quality Over Quantity

The framework optimizes for:

    Evidence Quality


not:

    Number of Tools


and:

    Number of Findings


not:

    Number of Alerts


A single validated vulnerability
is more valuable than:

    100 unvalidated alerts.


# 72. Final Selection Algorithm

The Orchestrator should conceptually follow:

    RECEIVE REQUEST
          |
          v
    DEFINE SCOPE
          |
          v
    IDENTIFY OBJECTIVE
          |
          v
    IDENTIFY TARGET LAYER
          |
          v
    DETERMINE REQUIRED EVIDENCE
          |
          v
    CHECK AUTHORIZATION
          |
          v
    CHECK ENVIRONMENT
          |
          v
    SELECT AGENT
          |
          v
    SELECT TOOL
          |
          v
    DEFINE EXECUTION PLAN
          |
          v
    EXECUTE
          |
          v
    COLLECT EVIDENCE
          |
          v
    CORRELATE
          |
          v
    VALIDATE
          |
          v
    REPORT


# 73. Final Decision Tree

Use this simplified decision tree:

    Is the objective source-code analysis?
        |
       YES
        |
        v
      Semgrep


    NO
        |
        v
    Is the objective functional / E2E testing?
        |
       YES
        |
        v
    TestSprite


    NO
        |
        v
    Is the objective runtime security testing?
        |
       YES
        |
        v
    pentest-ai / PentesterFlow


    NO
        |
        v
    Is cross-source correlation required?
        |
       YES
        |
        v
    Vulnerability Analyst


    NO
        |
        v
    Is the objective reporting?
        |
       YES
        |
        v
    Report Generator


    NO
        |
        v
    Re-evaluate the task before selecting a tool.


# 74. Final Framework Principle

The framework is not:

    Claude
        +
    Four MCP Tools


It is:

    Claude Code
        |
        v
    Security Orchestrator
        |
        v
    Intelligent Tool Selection
        |
        +------------------------+
        |                        |
        v                        v
    Static Analysis        Dynamic Security
        |                        |
      Semgrep              pentest-ai
                              /
                       PentesterFlow
        |
        +------------------------+
                                 |
                                 v
                           QA Testing
                                 |
                             TestSprite
                                 |
                                 v
                      Vulnerability Analyst
                                 |
                                 v
                         Report Generator


The objective is not:

    "Use all available tools."


The objective is:

    "Use the minimum appropriate tools
     to produce the strongest reliable evidence."


The framework must optimize for:

    Correct Tool
        +
    Correct Agent
        +
    Correct Evidence
        +
    Correct Validation
        +
    Correct Reporting


Final rule:

    Scope determines what may be tested.

    Objective determines what must be proven.

    Evidence determines which tool is appropriate.

    The Agent determines how the tool is used.

    The Vulnerability Analyst determines whether
    security evidence constitutes a validated finding.

    The Report Generator communicates the validated result.

    No tool output alone is a final security conclusion.