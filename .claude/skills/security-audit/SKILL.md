---
name: security-audit
description: Execute a structured, authorized security audit across application code, APIs, infrastructure, and exposed services by coordinating reconnaissance, static analysis, dynamic testing, vulnerability validation, risk assessment, and reporting through the framework's agents, rules, knowledge, and MCP tools.
---

# Security Audit Skill

## 1. Purpose

This skill defines the operational workflow for performing a complete security audit within the AI Security & QA Engineering Framework.

The skill is responsible for transforming an authorized security assessment from:

    Scope
        |
        v
    Reconnaissance
        |
        v
    Analysis
        |
        v
    Security Testing
        |
        v
    Validation
        |
        v
    Risk Assessment
        |
        v
    Reporting
        |
        v
    Remediation / Retesting


This skill does not replace the Security Orchestrator.

The Security Orchestrator decides:

    What needs to happen.

This skill defines:

    How a security audit should be executed.


# 2. Framework Relationship

The security audit operates inside the following architecture:

    Claude Code
         |
         v
    Security Orchestrator
         |
         v
    Security Audit Skill
         |
    +----+-------------------+------------------+
    |                        |                  |
    v                        v                  v
Security Agent       Code Review Agent      QA Agent
    |                        |                  |
    v                        v                  v
pentest-ai             Semgrep              TestSprite
PentesterFlow          Manual Review        QA Testing
    |                        |                  |
    +------------+-----------+------------------+
                 |
                 v
        Vulnerability Analyst
                 |
                 v
          Report Generator


The skill therefore acts as the operational workflow connecting:

    Orchestration

with:

    Security Execution


# 3. Required Framework Files

Before executing a security audit, use:

    .claude/CLAUDE.md

    .claude/agents/security-orchestrator.md
    .claude/agents/security-agent.md
    .claude/agents/code-review-agent.md
    .claude/agents/qa-agent.md
    .claude/agents/vulnerability-analyst.md
    .claude/agents/report-generator.md

    .claude/rules/tool-selection.md
    .claude/rules/severity-model.md
    .claude/rules/workflow.md

    .claude/knowledge/security-patterns.md
    .claude/knowledge/common-vulnerabilities.md
    .claude/knowledge/testing-strategy.md


These files provide:

    Architecture
    Agent responsibilities
    Tool selection
    Severity policy
    Workflow policy
    Security knowledge
    Testing strategy


Do not contradict them.


# 4. Authorization Gate

Before performing active security testing, establish that the target is authorized and in scope.

Required:

    Authorized Target
    Defined Scope
    Permitted Testing
    Relevant Environment
    Applicable Restrictions


If authorization or scope is unclear:

    STOP ACTIVE TESTING.


Do not assume authorization.


# 5. Scope Definition

Create an explicit scope model.

Capture:

    Applications
    Domains
    APIs
    IP Addresses
    Services
    Repositories
    Mobile Applications
    Cloud Resources
    Environments
    User Roles
    Testing Windows


Also record:

    Out-of-Scope Assets
    Restricted Actions
    Production Constraints
    Data Handling Restrictions


# 6. Scope Classification

Classify assets as:

    In Scope
    Out of Scope
    Unknown


Rules:

    In Scope
        -> May be assessed according to authorization.

    Out of Scope
        -> Must not be tested.

    Unknown
        -> Do not perform active testing until clarified.


# 7. Audit Modes

The skill supports multiple audit modes.

### Full Security Audit

Use when the objective is broad application security assessment.

Includes:

    Reconnaissance
    Static Analysis
    Dynamic Testing
    API Testing
    Authentication Testing
    Authorization Testing
    Input Validation
    Configuration Review
    Vulnerability Validation
    Reporting


### Code Security Audit

Focus on:

    Source Code
    Dependencies
    Authentication
    Authorization
    Input Handling
    Secrets
    Injection Risks
    Cryptography
    Business Logic


### API Security Audit

Focus on:

    Authentication
    Authorization
    Object-Level Access
    Input Validation
    Rate Limiting
    Data Exposure
    Business Logic
    API Configuration


### Web Application Audit

Focus on:

    Authentication
    Authorization
    Session Management
    Input Validation
    Injection
    Client-Side Security
    Server-Side Security
    Business Logic


### Targeted Audit

Focus only on the explicitly requested security area.


# 8. Audit Lifecycle

Use the following lifecycle:

    01. Authorization
          |
          v
    02. Scope Definition
          |
          v
    03. Asset Discovery
          |
          v
    04. Application Understanding
          |
          v
    05. Static Analysis
          |
          v
    06. Dynamic Testing
          |
          v
    07. Functional / QA Testing
          |
          v
    08. Finding Correlation
          |
          v
    09. Vulnerability Validation
          |
          v
    10. Risk Assessment
          |
          v
    11. Reporting
          |
          v
    12. Remediation
          |
          v
    13. Retesting


Not every audit requires every phase.

The Orchestrator determines which phases are relevant.


# 9. Phase 1 — Authorization

Confirm:

    Target
    Scope
    Permissions
    Testing Type
    Constraints


Output:

    Authorized Assessment Context


If the authorization state cannot be established:

    Do not continue with active testing.


# 10. Phase 2 — Scope Definition

Build a scope inventory.

Example:

    Application:
        Example Web Application

    APIs:
        /api/v1/*
        /api/v2/*

    Environment:
        Staging

    Roles:
        Guest
        User
        Admin


Record assumptions explicitly.


# 11. Phase 3 — Reconnaissance

Understand the target before testing aggressively.

Identify:

    Technologies
    Frameworks
    Endpoints
    APIs
    Authentication Mechanisms
    User Roles
    Services
    Dependencies
    Entry Points
    Trust Boundaries


Use authorized tools only.


# 12. Reconnaissance Principle

The objective is:

    Understand the Attack Surface.


Not:

    Generate as many scanner results as possible.


Prioritize:

    High-Value Assets
    Authentication Boundaries
    Authorization Boundaries
    Sensitive Data
    Administrative Interfaces
    External Integrations
    High-Risk Input Paths


# 13. Application Understanding

Before testing, establish:

    How users authenticate.
    How authorization works.
    How sessions are managed.
    How data flows.
    Where trust boundaries exist.
    Which services communicate.
    Where sensitive operations occur.


Build a mental model:

    User
      |
      v
    Frontend
      |
      v
    API
      |
      v
    Services
      |
      v
    Database


Then identify:

    Trust Boundaries
    Privilege Boundaries
    Data Boundaries


# 14. Attack Surface Mapping

Identify:

    Authentication Endpoints
    Authorization Checks
    File Uploads
    Search
    Query Parameters
    JSON Bodies
    Headers
    Cookies
    Webhooks
    Admin Functions
    Export Functions
    Import Functions
    Third-Party Integrations


Prioritize security-sensitive entry points.


# 15. Phase 4 — Static Analysis

Static analysis is performed through the Code Review Agent.

Primary tool:

    Semgrep


Potential analysis areas:

    Injection
    Authentication
    Authorization
    Secrets
    Cryptography
    File Handling
    Command Execution
    Deserialization
    SSRF
    Path Traversal
    Security Misconfiguration
    Unsafe Dependencies


Static findings are:

    Candidate Findings


They are not automatically vulnerabilities.


# 16. Static Analysis Workflow

Use:

    Source Code
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
        |
        v
    Validation


Do not bypass the Vulnerability Analyst.


# 17. Phase 5 — Dynamic Security Testing

Dynamic security testing is performed by:

    Security Agent


Relevant tools may include:

    pentest-ai
    PentesterFlow


Tool selection must follow:

    .claude/rules/tool-selection.md


Do not use every tool automatically.


## PentesterFlow Usage

When selected by the Security Agent, PentesterFlow is used for broader dynamic security workflows, including multi-stage testing, attack-chain validation, and deeper adversarial validation.

PentesterFlow is a standalone CLI/TUI capability, not an MCP server.

The Security Agent owns PentesterFlow invocation, configuration, and capability-specific failure handling.

Refer to:
.claude/agents/security-agent.md

Selection guidance is defined in:
.claude/rules/tool-selection.md

Do not assume a non-interactive/headless execution path exists.

PentesterFlow's own --resume session mechanism is separate from the framework's engagement checkpoint/resume mechanism.

For every PentesterFlow execution, preserve:
- execution status
- tool failures
- evidence
- limitations
- coverage impact

PentesterFlow output is candidate security evidence only.
Do not treat tool output as a final vulnerability conclusion.
Route security findings to the Vulnerability Analyst.

Do not duplicate:
- PentesterFlow CLI invocation syntax
- backend/model configuration
- executable path/version
- full flag documentation

Those belong to:
.claude/agents/security-agent.md


# 18. Dynamic Testing Principle

Use the least invasive test capable of establishing the security property.

Prefer:

    Safe Validation

over:

    Maximum Exploitation


The goal is to establish:

    Whether the security control works.


Not:

    How much damage can be caused.


# 19. Dynamic Testing Areas

Depending on scope:

    Authentication
    Authorization
    Session Management
    Input Validation
    Injection
    File Upload
    Path Traversal
    SSRF
    Business Logic
    API Security
    Access Control
    Configuration
    Information Disclosure
    Security Headers
    Rate Limiting


Only test applicable areas.


# 20. Authentication Testing

Evaluate:

    Login
    Logout
    Password Recovery
    Session Creation
    Session Termination
    MFA
    Token Handling
    Account Enumeration
    Authentication Bypass


Questions:

    Can authentication be bypassed?

    Are sessions protected?

    Are tokens validated correctly?

    Can authentication state be confused?


# 21. Authorization Testing

Evaluate:

    Horizontal Access Control
    Vertical Access Control
    Object-Level Authorization
    Function-Level Authorization
    Tenant Isolation


Test across appropriate roles:

    Anonymous
    User
    Privileged User
    Administrator


Do not access unauthorized real-world data.


# 22. Input Validation

Identify attacker-controlled inputs:

    Parameters
    JSON
    Forms
    Headers
    Cookies
    Files
    URLs
    Webhooks


Determine whether unsafe input reaches sensitive operations.


# 23. Injection Testing

Potential classes include:

    SQL Injection
    NoSQL Injection
    Command Injection
    Template Injection
    LDAP Injection
    XPath Injection
    Expression Injection


Validate safely.

Do not cause destructive database or system operations merely to prove exploitability.


# 24. File Handling

Evaluate:

    Upload
    Download
    File Names
    MIME Types
    Path Handling
    Archive Extraction
    File Storage
    Access Control


Look for:

    Path Traversal
    Arbitrary File Access
    Unsafe Upload
    File Type Confusion


# 25. API Security

Evaluate:

    Authentication
    Authorization
    Object-Level Authorization
    Function-Level Authorization
    Input Validation
    Mass Assignment
    Excessive Data Exposure
    Rate Limiting
    Error Handling
    Versioning


Map:

    Endpoint
        |
        v
    Role
        |
        v
    Object
        |
        v
    Expected Authorization


# 26. Business Logic

Do not limit the audit to technical scanner findings.

Evaluate:

    Workflow Bypass
    State Manipulation
    Price Manipulation
    Privilege Abuse
    Approval Bypass
    Race Conditions
    Transaction Abuse
    Repeated Actions
    Missing Limits


Business logic vulnerabilities often require:

    Context
    Manual Reasoning
    Multi-Step Testing


# 27. Phase 6 — QA / Functional Testing

The QA Agent uses:

    TestSprite


when appropriate.

The purpose is to evaluate:

    Functional Behavior
    Regression
    API Behavior
    E2E Behavior
    User Flows


Security-relevant failures should be correlated with security testing.


# 28. QA Security Correlation

Example:

    TestSprite:
        User can access another user's profile.

    Security Agent:
        Authorization endpoint accepts foreign object ID.

    Code Review:
        Ownership check missing.


These observations may represent:

    One Security Finding


The Vulnerability Analyst performs the correlation.


# 29. Functional Defect Separation

Not every QA failure is a vulnerability.

Example:

    Button crashes page.

Usually:

    Functional Defect


Example:

    Button reveals another user's private information.

Potentially:

    Security Vulnerability


The distinction must be preserved.


# 30. Phase 7 — Finding Correlation

All candidate findings converge into:

    Vulnerability Analyst


Flow:

    Semgrep
        |
    pentest-ai
        |
    PentesterFlow
        |
    TestSprite
        |
    Manual Analysis
        |
        v
    Vulnerability Analyst


The analyst performs:

    Normalization
    Correlation
    Deduplication
    Validation
    Root Cause Analysis
    Impact Analysis
    Severity
    Confidence


# 31. Finding Normalization

Normalize candidate observations into:

    Finding ID
    Source
    Asset
    Component
    Vulnerability Type
    Description
    Evidence
    Reproduction
    Impact
    Confidence
    Initial Severity
    Related Findings


Do not discard original evidence.


# 32. Deduplication

Correlate findings when they share:

    Root Cause
    Attack Path
    Security Boundary
    Component
    Impact


Multiple tools reporting one issue should normally result in:

    One Finding
        +
    Multiple Evidence Sources


# 33. Independent Validation

The Vulnerability Analyst determines:

    Validated
    False Positive
    Not Exploitable
    Inconclusive
    Duplicate


Do not treat tool confidence as final truth.


# 34. Phase 8 — Risk Assessment

Validated findings are assessed using:

    .claude/rules/severity-model.md


Evaluate:

    Impact
    Exploitability
    Attacker Capability
    Privileges
    User Interaction
    Scope
    Business Context
    Existing Controls


Maintain separation between:

    Severity

and:

    Confidence


# 35. Severity Principle

Use:

    Severity =
        Impact + Exploitability + Context


Use:

    Confidence =
        Strength of Evidence


Never:

    Severity = Confidence


# 36. Attack Chain Analysis

Look for validated chains:

    Initial Access
        |
        v
    Information Disclosure
        |
        v
    Credential Access
        |
        v
    Privilege Escalation
        |
        v
    Sensitive Data Access


Only build chains supported by evidence.


# 37. Attack Chain Importance

A moderate finding may become strategically important if it enables:

    Authentication Bypass
    Privilege Escalation
    Sensitive Data Access
    Administrative Access


The Vulnerability Analyst should communicate this to the Report Generator.


# 38. Phase 9 — Reporting

Validated findings are passed to:

    .claude/agents/report-generator.md


The Report Generator creates:

    Executive Summary
    Technical Findings
    Risk Summary
    Attack Chains
    Remediation Priorities
    Retest Status


The Report Generator does not rediscover findings.


# 39. Report Integrity

The final report must preserve:

    Finding IDs
    Severity
    Confidence
    Evidence
    Root Cause
    Impact
    Status


Do not modify validated conclusions without returning to the Vulnerability Analyst.


# 40. Phase 10 — Remediation

For each finding:

    Identify Root Cause
        |
        v
    Recommend Fix
        |
        v
    Implement Fix
        |
        v
    Retest


Remediation should address:

    Root Cause


not only:

    Proof-of-Concept Input


# 41. Phase 11 — Retesting

Retesting should reproduce:

    Original Attack


Then verify:

    Expected Security Boundary


Finally test:

    Relevant Adjacent Paths


Possible states:

    Verified Fixed
    Partially Fixed
    Not Fixed
    Retest Inconclusive


# 42. Audit Completion Criteria

A security audit is complete when:

    [ ] Authorization confirmed
    [ ] Scope defined
    [ ] Attack surface understood
    [ ] Relevant analysis completed
    [ ] Relevant dynamic testing completed
    [ ] Relevant QA testing completed
    [ ] Findings correlated
    [ ] Findings validated
    [ ] Severity assigned
    [ ] Confidence assigned
    [ ] Attack chains analyzed
    [ ] Report generated
    [ ] Remediation status recorded
    [ ] Retest completed when applicable


Not every audit requires remediation or retesting during the same execution.


# 43. Tool Selection

Never select tools arbitrarily.

Use:

    .claude/rules/tool-selection.md


General mapping:

    Static Code Analysis
        ->
    Semgrep

    Dynamic Security Testing
        ->
    pentest-ai / PentesterFlow

    Functional / E2E Testing
        ->
    TestSprite

    Correlation / Validation
        ->
    Vulnerability Analyst

    Reporting
        ->
    Report Generator


The Orchestrator remains responsible for deciding which capability is needed.


# 44. Tool Failure Handling

If a tool fails:

    Do not fabricate results.


Record:

    Tool
    Operation
    Failure
    Impact on Coverage


Then determine whether another authorized method can provide equivalent evidence.


# 45. Tool Redundancy

Do not run multiple tools merely because they exist.

Use multiple tools when they provide:

    Independent Evidence
    Different Coverage
    Complementary Analysis


Example:

    Semgrep
        +
    Dynamic Testing


is valuable because:

    Static Evidence
        +
    Runtime Evidence


can strengthen validation.


# 46. Coverage Tracking

Track:

    Assets Tested
    Endpoints Tested
    Roles Tested
    Security Areas Tested
    Tools Used
    Areas Not Tested
    Testing Limitations


A report should never imply complete coverage if significant areas were not assessed.


# 47. Testing Limitations

Document limitations such as:

    No Production Access
    Limited Credentials
    Restricted API Endpoints
    Time Constraints
    Tool Failure
    Unavailable Environment
    Out-of-Scope Components


Limitations affect confidence and coverage.


# 48. Evidence Management

For every candidate finding preserve:

    Original Source
    Tool
    Timestamp when available
    Target
    Evidence
    Reproduction
    Analyst Decision


Maintain traceability from:

    Finding

to:

    Evidence.


# 49. Sensitive Data Handling

Never unnecessarily expose:

    Passwords
    API Keys
    Tokens
    Private Keys
    Personal Data
    Production Secrets


Redact sensitive information before:

    Reports
    Logs
    Shared Artifacts


# 50. Safe Testing Principle

Use:

    Minimum Necessary Impact


Avoid:

    Destructive Testing
    Data Deletion
    Service Disruption
    Resource Exhaustion
    Account Lockouts
    Production Damage


unless explicitly authorized and necessary.


# 51. Stop Conditions

Stop active testing when:

    Authorization is unclear.

    Scope is exceeded.

    Testing becomes destructive.

    Production impact becomes likely.

    Sensitive real-world data could be unnecessarily exposed.

    Further testing provides no meaningful additional evidence.


Escalate to the Security Orchestrator.


# 52. Finding Quality Gate

Before a candidate reaches reporting:

    [ ] In Scope
    [ ] Security Relevant
    [ ] Evidence Exists
    [ ] Reproduction Exists or Strong Evidence Exists
    [ ] Root Cause Identified
    [ ] Impact Understood
    [ ] Duplicate Check Completed
    [ ] Severity Justified
    [ ] Confidence Justified
    [ ] Sensitive Evidence Redacted


# 53. Audit Quality Gate

Before completing the audit:

    [ ] Scope matches authorization
    [ ] No unauthorized testing occurred
    [ ] Tool failures documented
    [ ] Coverage limitations documented
    [ ] Findings validated
    [ ] False positives excluded
    [ ] Duplicates correlated
    [ ] Severity follows framework policy
    [ ] Reports contain no unsupported claims
    [ ] Sensitive information is protected


# 54. Orchestrator Interaction

The Security Orchestrator may:

    Start Audit
    Pause Audit
    Resume Audit
    Expand Scope
    Narrow Scope
    Request Additional Validation
    Request Retesting
    Request Reporting


The skill should respond to the Orchestrator's workflow state.

Do not independently expand scope.


# 55. Agent Interaction

The audit skill coordinates with:

    Security Agent
        ->
    Dynamic Security Testing

    Code Review Agent
        ->
    Static Analysis

    QA Agent
        ->
    Functional / E2E Testing

    Vulnerability Analyst
        ->
    Validation / Correlation

    Report Generator
        ->
    Reporting


Each agent remains responsible for its own domain.


# 56. Knowledge Interaction

Use:

    .claude/knowledge/security-patterns.md

for known security patterns.

Use:

    .claude/knowledge/common-vulnerabilities.md

for vulnerability-specific knowledge.

Use:

    .claude/knowledge/testing-strategy.md

for testing methodology and prioritization.


Knowledge supports analysis.

It does not replace evidence.


# 57. Rules Interaction

Use:

    .claude/rules/tool-selection.md

to determine appropriate tools.

Use:

    .claude/rules/severity-model.md

to determine severity.

Use:

    .claude/rules/workflow.md

to determine workflow behavior.


Do not duplicate these rules here unless necessary for operational clarity.


# 58. Audit State

Maintain a clear state:

    INITIALIZING
        |
        v
    SCOPING
        |
        v
    RECON
        |
        v
    ANALYSIS
        |
        v
    TESTING
        |
        v
    VALIDATION
        |
        v
    RISK_ASSESSMENT
        |
        v
    REPORTING
        |
        v
    REMEDIATION
        |
        v
    RETEST
        |
        v
    COMPLETE


Some engagements may end before:

    REMEDIATION

or:

    RETEST


# 59. Audit State Integrity

Never claim:

    COMPLETE


while required phases remain unresolved.

If a phase was skipped:

    Record why.


Example:

    Dynamic Testing:
        Skipped

    Reason:
        No authorized runtime environment available.


# 60. Coverage vs Completion

An audit can be:

    Complete Within Scope

without being:

    Complete Across Everything.


Always distinguish:

    Scope Completion

from:

    Universal Security Assurance


The framework must never imply that testing proves the application is completely secure.


# 61. Security Assurance Boundary

The final conclusion should communicate:

    What was tested.

    What was not tested.

    What was found.

    What was validated.

    What remains uncertain.


Never state:

    "The application is secure."


Prefer:

    "No validated vulnerabilities were identified within
    the tested scope and assessment constraints."


# 62. Failure Recovery

If the workflow fails:

    Preserve existing evidence.

    Preserve validated findings.

    Record the failed phase.

    Resume from the last safe state.


Do not restart blindly and create duplicate findings.


# 63. Resume Workflow

When resuming an interrupted audit:

    Load:
        Scope
        Existing Findings
        Tool Results
        Validation State
        Coverage
        Outstanding Tasks


Then continue from:

    Last Valid State


Do not repeat destructive or unnecessary tests.


# 64. Audit Handoff

When handing work between agents, preserve:

    Scope
    Objective
    Current State
    Findings
    Evidence
    Outstanding Questions
    Restrictions


No agent should need to reconstruct the entire engagement from scratch.


# 65. Final Audit Output

The final audit should provide:

    Assessment Summary
    Scope
    Coverage
    Methodology
    Validated Findings
    Severity Distribution
    Attack Chains
    Remediation Priorities
    Limitations
    Retest Status


The exact report format is controlled by:

    .claude/agents/report-generator.md


# 66. Final Operating Principle

The security audit must follow:

    Scope
        ->
    Understand
        ->
    Test
        ->
    Correlate
        ->
    Validate
        ->
    Assess
        ->
    Report
        ->
    Remediate
        ->
    Retest


Never:

    Scan
        ->
    Assume
        ->
    Report


The framework's core philosophy is:

    Tools discover.

    Agents analyze.

    The Vulnerability Analyst validates.

    The Report Generator communicates.

    The Orchestrator controls the workflow.


The Security Audit Skill exists to make these responsibilities operate as one coherent system.

Its goal is not to maximize:

    Tool Count

or:

    Finding Count


Its goal is to maximize:

    Security Coverage
        +
    Evidence Quality
        +
    Validation Accuracy
        +
    Actionable Results


while maintaining:

    Scope Discipline
    Safety
    Traceability
    Consistency
    Professional Reporting.