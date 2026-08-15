---
name: reporting
description: Define the reporting workflow for converting correlated and validated security, code-review, and QA results into accurate, evidence-based, professional security reports.
---

# Reporting Skill

## 1. Purpose

This skill defines the reporting layer of the AI Security & QA Engineering Framework.

Its responsibility is to transform:

    Raw Observations
        |
        v
    Tool Results
        |
        v
    Agent Findings
        |
        v
    Correlated Evidence
        |
        v
    Validated Findings
        |
        v
    Professional Report


The Reporting Skill does not perform:

    Security Testing
    Source-Code Review
    QA Execution
    Vulnerability Validation


Those responsibilities belong to specialized components.


The reporting responsibility is:

    Accurate Representation
        +
    Evidence Preservation
        +
    Finding Organization
        +
    Risk Communication
        +
    Actionable Remediation


# 2. Framework Position

The reporting layer sits at the end of the framework pipeline:

    Claude Code
        |
        v
    Security Orchestrator
        |
        +-------------------+
        |                   |
        v                   v
    Security Agent     Code Review Agent
        |                   |
        v                   v
    Dynamic Findings   Static Findings
        |                   |
        +---------+---------+
                  |
                  v
             QA Agent
                  |
                  v
             QA Results
                  |
                  v
        Vulnerability Analyst
                  |
                  v
        Correlated Findings
                  |
                  v
         Validated Findings
                  |
                  v
         Report Generator
                  |
                  v
         Reporting Skill
                  |
                  v
        Professional Report


The Report Generator owns report generation.

This skill defines the reporting standards,
structure, quality gates, and output rules.


# 3. Core Principle

A report must represent:

    What was actually tested
    What was actually observed
    What was actually validated


Never represent:

    Assumption

as:

    Fact


Never represent:

    Potential Vulnerability

as:

    Confirmed Vulnerability


Never represent:

    Tool Output

as:

    Final Security Conclusion


# 4. Source of Truth

The primary source of truth for security findings is:

    Vulnerability Analyst


The Vulnerability Analyst receives evidence from:

    Security Agent
    Code Review Agent
    QA Agent
    Security Tools
    Application Analysis


The Report Generator consumes:

    Validated Findings


It must not independently promote unvalidated observations
into confirmed vulnerabilities.


# 5. Reporting Pipeline

Use:

    Collect
        |
        v
    Normalize
        |
        v
    Correlate
        |
        v
    Validate
        |
        v
    Classify
        |
        v
    Prioritize
        |
        v
    Structure
        |
        v
    Generate
        |
        v
    Quality Review
        |
        v
    Deliver


# 6. Report Types

The framework may generate:

    Security Assessment Report
    Penetration Testing Report
    Application Security Report
    Code Security Review Report
    QA / Testing Report
    Vulnerability Assessment Report
    Executive Security Summary
    Technical Findings Report
    Remediation Report
    Regression Validation Report


The Orchestrator determines the required report type.


# 7. Report Audience

Identify the intended audience.

Possible audiences:

    Executive Leadership
    Security Team
    Engineering Team
    Developers
    QA Team
    DevOps
    Product Team
    Client


A technical report should not assume that
every reader has security expertise.


# 8. Executive vs Technical Reporting

Executive reporting should emphasize:

    Business Impact
    Overall Risk
    Critical Findings
    Major Exposure Areas
    Remediation Priorities


Technical reporting should emphasize:

    Root Cause
    Attack Path
    Evidence
    Reproduction
    Affected Components
    Remediation


Do not overload executive summaries with:

    Raw Tool Output
    Stack Traces
    Internal Debug Logs


# 9. Standard Report Structure

A professional security report should normally contain:

    1. Executive Summary
    2. Assessment Overview
    3. Scope
    4. Methodology
    5. Testing Coverage
    6. Risk Summary
    7. Findings
    8. Technical Evidence
    9. Remediation Guidance
    10. Validation / Regression Results
    11. Limitations
    12. Conclusion
    13. Appendix


Not every report requires every section.


# 10. Executive Summary

The executive summary should answer:

    What was assessed?
    Why was it assessed?
    What was discovered?
    How serious is the exposure?
    What should be fixed first?


Keep it concise.

Do not include unsupported claims.


# 11. Assessment Overview

Include:

    Assessment Name
    Target
    Assessment Type
    Environment
    Assessment Period
    Testing Approach
    Framework Version when relevant


Avoid exposing:

    Secrets
    Credentials
    API Keys
    Tokens


# 12. Scope

Clearly define:

    In-Scope Assets
    In-Scope Applications
    In-Scope APIs
    In-Scope Features
    In-Scope Roles


Also define:

    Out-of-Scope Assets
    Out-of-Scope Features


A report must never imply testing coverage outside
the actual scope.


# 13. Methodology

Describe the methodology used.

Possible components:

    Reconnaissance
    Application Mapping
    Static Analysis
    Dynamic Testing
    API Testing
    Functional Testing
    E2E Testing
    Authentication Testing
    Authorization Testing
    Business Logic Testing
    Vulnerability Validation
    Regression Testing


Only include techniques actually performed.


# 14. Tool Disclosure

Where appropriate, document tools used.

Examples:

    Semgrep
    pentest-ai
    PentesterFlow
    TestSprite


Tool names should explain methodology,
not replace findings.


Avoid statements such as:

    "Tool X found 20 vulnerabilities."


Prefer:

    "Automated analysis identified candidate issues,
     which were subsequently reviewed and validated."


# 15. Tool Output vs Finding

A tool result is:

    Observation


A validated vulnerability is:

    Finding


The distinction must remain explicit.


Example:

    Tool:
        Potential SQL Injection


does not automatically become:

    Confirmed SQL Injection


until validation establishes:

    Root Cause
    Exploitability
    Impact
    Evidence


# 16. Finding Identity

Each validated finding should have a unique identifier.

Recommended format:

    SEC-001
    SEC-002
    SEC-003


For larger engagements:

    APP-SEC-001
    API-SEC-001


IDs must remain stable throughout the report.


# 17. Finding Title

Finding titles should be:

    Specific
    Concise
    Actionable


Good:

    "Broken Object-Level Authorization Exposes Other Users' Records"


Bad:

    "Authorization Issue"


Avoid sensational language.


# 18. Finding Severity

Every validated security finding should include:

    Severity


Use the framework severity model defined in:

    .claude/rules/severity-model.md


Do not invent a separate severity system inside this skill.


# 19. Severity Consistency

Severity must be consistent across:

    Vulnerability Analyst
    Report Generator
    Reporting Skill


If severity is disputed:

    Follow severity-model.md


and:

    Document the rationale.


# 20. Risk Summary

Provide an overview such as:

    Critical: X
    High: X
    Medium: X
    Low: X
    Informational: X


Only count:

    Validated Findings


Do not count:

    Raw Tool Alerts
    Duplicates
    False Positives
    Inconclusive Observations


# 21. Finding Structure

Each finding should normally contain:

    Finding ID
    Title
    Severity
    Confidence
    Affected Component
    Description
    Impact
    Evidence
    Reproduction
    Root Cause
    Attack Scenario
    Remediation
    References when appropriate
    Validation Status


# 22. Confidence

Where useful, distinguish:

    High Confidence
    Medium Confidence
    Low Confidence


Confidence is not severity.


Example:

    High Severity
    Medium Confidence


is valid.


Severity answers:

    "How bad is it?"


Confidence answers:

    "How certain are we?"


# 23. Description

The description should explain:

    What is wrong?
    Where is it?
    Why does it matter?


Keep the description technical enough
for engineers to understand the issue.


# 24. Impact

Impact should explain consequences.

Consider:

    Confidentiality
    Integrity
    Availability
    Authentication
    Authorization
    Privacy
    Financial Impact
    Business Impact


Avoid vague statements such as:

    "This is dangerous."


Explain:

    What an attacker could actually achieve.


# 25. Evidence

Evidence must support the finding.

Possible evidence:

    HTTP Request
    HTTP Response
    Screenshot
    Source Code
    Stack Trace
    Log
    Test Result
    Tool Output
    Reproduction Result


Only include evidence necessary to demonstrate the issue.


# 26. Evidence Redaction

Before including evidence:

    Remove API Keys
    Remove Passwords
    Remove Session Tokens
    Remove Private Keys
    Remove Sensitive Personal Data
    Remove Unnecessary Secrets


Never publish secrets simply because they appeared
during testing.


# 27. Reproduction

A reproduction procedure should be:

    Clear
    Minimal
    Deterministic
    Authorized
    Safe


A good reproduction contains:

    Preconditions
    Authentication State
    Request / Action
    Input
    Expected Result
    Actual Result


# 28. Root Cause

Where possible identify:

    Code Defect
    Configuration Error
    Architecture Problem
    Missing Validation
    Missing Authorization
    Unsafe Dependency Usage
    Design Weakness
    Process Failure


Avoid speculative root causes.


If root cause cannot be confirmed:

    State that clearly.


# 29. Attack Scenario

Explain:

    Attacker Capability
        |
        v
    Initial Condition
        |
        v
    Exploit
        |
        v
    Result
        |
        v
    Business / Security Impact


Keep scenarios realistic.


# 30. Remediation

Remediation should be actionable.

Good remediation includes:

    What to change
    Where to change it
    Why it fixes the issue
    What behavior should be enforced


Avoid:

    "Improve security."


Prefer:

    "Enforce server-side ownership checks before
     returning the requested object."


# 31. Remediation Priority

Prioritize remediation using:

    Severity
    Exploitability
    Exposure
    Business Impact
    Asset Criticality
    Attack Preconditions


Suggested priority:

    Immediate
    High
    Planned
    Monitor


Use the framework severity model where applicable.


# 32. Fix Verification

When a finding has been remediated:

    Original Finding
        |
        v
    Fix Applied
        |
        v
    Regression Test
        |
        v
    Security Re-test
        |
        v
    Validation Result


Possible statuses:

    Fixed
    Partially Fixed
    Not Fixed
    Cannot Verify
    Reopened


# 33. Regression Evidence

A remediation report should include:

    Original Finding ID
    Fix Description
    Original Reproduction
    Retest Result
    Regression Result
    Remaining Risk


Never claim:

    "Fixed"


without appropriate evidence.


# 34. Cross-Agent Correlation

Findings may originate from:

    Security Agent
    Code Review Agent
    QA Agent


The Vulnerability Analyst correlates them.

Example:

    Static Analysis
        |
        v
    Missing Authorization Check

    QA
        |
        v
    Unauthorized Access Observed

    Dynamic Testing
        |
        v
    Exploitation Confirmed


These should normally become:

    One Correlated Finding


with:

    Multiple Evidence Sources


# 35. Duplicate Handling

Do not report duplicate findings separately
when they represent the same:

    Root Cause
    Attack Path
    Component
    Impact


Instead:

    Merge Evidence


and:

    Preserve Relevant Sources.


# 36. Related Findings

Different findings may be related without being duplicates.

Example:

    Missing Authorization
        +
    Excessive Privilege


These may remain:

    Separate Findings


if their:

    Root Causes
    Impacts
    Remediation


are meaningfully different.


# 37. False Positives

False positives must not appear as confirmed findings.

They may be recorded in:

    Validation Notes
    Appendix
    Internal Assessment Artifacts


depending on the report type.


# 38. Inconclusive Findings

An inconclusive observation should be labeled:

    Inconclusive


and should include:

    Why Validation Failed
    What Evidence Exists
    What Is Needed for Confirmation


Never silently promote it.


# 39. Informational Observations

Informational observations may include:

    Hardening Opportunities
    Architecture Notes
    Security Recommendations
    Best Practices


Do not inflate informational items
into vulnerabilities.


# 40. Limitations

Every assessment report should document meaningful limitations.

Examples:

    Limited Credentials
    Unavailable Source Code
    Restricted Environment
    Inaccessible Endpoint
    Missing Test Data
    Third-Party Dependency
    Time Constraints
    Rate Limits


Limitations protect report integrity.


# 41. Coverage Statement

Use precise language.

Good:

    "Testing covered the authenticated API workflows
     identified during the assessment."


Bad:

    "The entire application was fully tested."


unless that statement is demonstrably true.


# 42. Testing Statistics

Where useful include:

    Endpoints Tested
    Workflows Tested
    Test Cases Executed
    Security Findings
    Code Findings
    QA Failures
    Blocked Tests


Statistics must be derived from actual execution data.


# 43. QA Result Integration

QA results may appear as:

    Test Coverage
    Functional Findings
    Regression Results
    Security-Relevant Observations


A QA failure must not automatically appear
as a security vulnerability.


# 44. Code Review Integration

Code review findings may provide:

    Source Location
    Vulnerable Code Path
    Root Cause
    Data Flow


If the issue is validated dynamically,
combine the evidence.


# 45. Dynamic Security Integration

Dynamic testing may provide:

    Reproduction
    Exploitability
    Runtime Behavior
    Impact Evidence


Dynamic evidence should strengthen
validated findings.


# 46. Evidence Hierarchy

Prefer evidence in this general order:

    Reproduced Exploit
        >
    Verified Runtime Behavior
        >
    Verified Source-Code Evidence
        >
    Correlated Tool Evidence
        >
    Unverified Tool Alert


The hierarchy does not replace analyst judgment.


# 47. Reproducibility Standard

A strong finding should allow another qualified engineer
to understand:

    What happened
    Why it happened
    How it was reproduced
    How it should be fixed
    How the fix can be verified


# 48. Report Language

Use:

    Precise
    Neutral
    Professional
    Evidence-Based
    Actionable


Avoid:

    Emotional Language
    Fear-Based Language
    Unsupported Certainty
    Marketing Language
    Sensational Claims


# 49. Technical Accuracy

Before finalizing:

    Verify URLs
    Verify Endpoints
    Verify File Paths
    Verify Finding IDs
    Verify Severity
    Verify Counts
    Verify Reproduction Steps
    Verify Remediation


Never invent missing technical details.


# 50. Uncertainty Handling

When information is unavailable:

    Say so.


Examples:

    "Could not be verified in the available environment."

    "Source code for this component was unavailable."

    "Impact could not be fully established."


Uncertainty is preferable to fabricated precision.


# 51. Report Consistency

Ensure consistency between:

    Executive Summary
    Risk Summary
    Finding Table
    Detailed Findings
    Conclusion


Example:

If the report says:

    3 High Findings


the findings section must contain:

    Exactly 3 validated High findings.


# 52. Finding Table

A summary table should normally include:

    ID
    Title
    Severity
    Affected Component
    Status


Example:

    | ID | Finding | Severity | Component | Status |
    |----|---------|----------|-----------|--------|
    | SEC-001 | ... | High | API | Open |
    | SEC-002 | ... | Medium | Web | Open |


# 53. Status Model

Recommended statuses:

    Open
    Fixed
    Partially Fixed
    Accepted Risk
    False Positive
    Inconclusive
    Reopened


Use only statuses supported by the engagement context.


# 54. Accepted Risk

If a finding is accepted as risk:

    Record:

        Finding ID
        Risk
        Business Owner
        Acceptance Context
        Date when available
        Remaining Risk


Do not silently remove the finding.


# 55. Reopened Findings

A finding should be reopened when:

    Previous Fix No Longer Works

or:

    Equivalent Exploitation Remains Possible


Record:

    Original Finding ID
    Regression Result
    New Evidence


# 56. Security Conclusion

The conclusion should summarize:

    Overall Security Posture
    Major Validated Risks
    Important Coverage Gaps
    Highest-Priority Actions


Do not claim:

    "Secure"


based solely on a limited assessment.


# 57. No Absolute Security Claims

Avoid statements such as:

    "The application is completely secure."

    "No vulnerabilities exist."

    "The system is immune to attack."


Prefer:

    "No additional vulnerabilities were identified
     within the defined scope and testing constraints."


# 58. Professional Report Quality Gate

Before delivery verify:

    [ ] Scope is accurate
    [ ] Methodology is accurate
    [ ] Tools listed were actually used
    [ ] Findings are validated
    [ ] Duplicate findings are consolidated
    [ ] Severity is consistent
    [ ] Evidence is sufficient
    [ ] Secrets are redacted
    [ ] Reproduction is reproducible
    [ ] Remediation is actionable
    [ ] Counts are correct
    [ ] Limitations are documented
    [ ] Security claims are supported
    [ ] QA results are correctly classified
    [ ] Regression status is accurate


# 59. Executive Quality Gate

Before finalizing the executive section:

    [ ] Business impact is understandable
    [ ] Critical risks are highlighted
    [ ] Priorities are clear
    [ ] No unsupported claims exist
    [ ] Technical noise is minimized


# 60. Technical Quality Gate

Before finalizing technical findings:

    [ ] Finding ID exists
    [ ] Title is specific
    [ ] Severity is correct
    [ ] Confidence is represented
    [ ] Affected component is known
    [ ] Description is accurate
    [ ] Impact is explained
    [ ] Evidence is included
    [ ] Reproduction is clear
    [ ] Root cause is supported
    [ ] Remediation is actionable
    [ ] Validation status is accurate


# 61. Report Generator Responsibilities

The Report Generator:

    Collects validated findings
    Organizes findings
    Builds report sections
    Produces summaries
    Formats technical evidence
    Produces remediation guidance
    Performs consistency checks


The Report Generator does not:

    Invent findings
    Invent evidence
    Upgrade severity without justification
    Validate vulnerabilities independently
    Claim tests were executed when they were not


# 62. Orchestrator Relationship

The Security Orchestrator decides:

    When reporting is required
    What report type is required
    Which findings should be included
    What audience should be targeted


The Report Generator executes:

    Report Construction


The Reporting Skill enforces:

    Reporting Standards


# 63. Vulnerability Analyst Relationship

The Vulnerability Analyst provides:

    Validated Findings
    Severity
    Confidence
    Evidence
    Root Cause
    Impact
    Validation Status


The Reporting Skill transforms this into:

    Human-Readable Security Communication


# 64. QA Relationship

The QA Agent provides:

    Test Coverage
    Test Results
    Functional Failures
    Regression Results
    Security-Relevant Observations


The Reporting Skill must preserve the distinction between:

    QA Failure

and:

    Validated Security Finding


# 65. Code Review Relationship

The Code Review Agent provides:

    Source-Level Findings
    Code Locations
    Data Flow
    Root Cause Candidates
    Static Evidence


These results may be correlated by:

    Vulnerability Analyst


before entering the final report.


# 66. Security Agent Relationship

The Security Agent provides:

    Dynamic Findings
    Runtime Evidence
    Attack Paths
    Reproduction Evidence


These results require:

    Validation


before becoming final security findings.


# 67. Tool Selection Relationship

Tool selection is governed by:

    .claude/rules/tool-selection.md


This skill must never redefine
the framework's tool-selection authority.


# 68. Severity Relationship

Severity is governed by:

    .claude/rules/severity-model.md


This skill must not create conflicting
severity definitions.


# 69. Workflow Relationship

The engagement lifecycle is governed by:

    .claude/rules/workflow.md


Reporting should respect:

    Engagement State
    Validation State
    Remediation State
    Closure State


# 70. Knowledge Relationship

Reporting may reference:

    .claude/knowledge/security-patterns.md

    .claude/knowledge/common-vulnerabilities.md

    .claude/knowledge/testing-strategy.md


Knowledge files provide:

    Context
    Patterns
    Interpretation


They do not replace:

    Engagement Evidence


# 71. Output Formats

Depending on the engagement, output may include:

    Markdown
    HTML
    PDF
    JSON
    CSV
    Structured Finding Data


The chosen format must preserve:

    Finding Identity
    Severity
    Evidence
    Remediation
    Status


# 72. Machine-Readable Findings

When producing structured output, use consistent fields.

Recommended structure:

    finding_id
    title
    severity
    confidence
    status
    asset
    component
    description
    impact
    evidence
    reproduction
    root_cause
    remediation
    validation
    sources


Do not silently change field semantics.


# 73. Markdown Reports

Markdown reports should prioritize:

    Readability
    Clear Headings
    Consistent Tables
    Code Blocks
    Evidence Sections
    Finding IDs


Avoid unnecessary decoration.


# 74. Evidence Formatting

Use appropriate formatting for:

    HTTP Requests
    HTTP Responses
    Source Code
    CLI Output
    JSON
    Logs


Example:

    ```http
    GET /api/resource/123
    Authorization: Bearer [REDACTED]
    ```


Secrets must remain redacted.


# 75. Screenshots

Screenshots should be included only when they add evidence.

Prefer screenshots showing:

    Relevant UI
    Error
    Unauthorized Access
    Unexpected Behavior


Avoid screenshots containing:

    Credentials
    Tokens
    Personal Data
    Unnecessary Sensitive Information


# 76. Report Versioning

Where applicable include:

    Report Version
    Assessment Version
    Application Version
    Framework Version


This helps track:

    Changes
    Retesting
    Remediation


# 77. Changelog

For updated reports, document:

    What Changed
    Why It Changed
    New Findings
    Closed Findings
    Reopened Findings
    Severity Changes


Do not silently rewrite historical conclusions.


# 78. Historical Integrity

Once a report is delivered:

    Preserve Original Findings


If new evidence changes the conclusion:

    Issue an Updated Version


Do not erase the original evidence trail.


# 79. Remediation Tracking

When supported, track:

    Finding
    Owner
    Recommended Fix
    Status
    Verification
    Remaining Risk


This allows the report to function as:

    Security Engineering Feedback


rather than only:

    Static Documentation


# 80. Final Reporting Pipeline

The complete framework should operate as:

    User Request
        |
        v
    Security Orchestrator
        |
        +---------------------+
        |                     |
        v                     v
    Security Agent      Code Review Agent
        |                     |
        v                     v
    Dynamic Evidence    Static Evidence
        |                     |
        +----------+----------+
                   |
                   v
               QA Agent
                   |
                   v
             QA Evidence
                   |
                   v
        Vulnerability Analyst
                   |
                   v
        Correlation + Validation
                   |
                   v
          Validated Findings
                   |
                   v
           Report Generator
                   |
                   v
          Reporting Skill
                   |
                   v
          Quality Validation
                   |
                   v
          Professional Report


# 81. Final Principle

The reporting layer exists to preserve the integrity
of the entire security workflow.

The framework should produce:

    Evidence
        +
    Validation
        +
    Context
        +
    Clear Risk
        +
    Actionable Remediation


A professional report is not:

    A list of tool alerts.


It is:

    A structured representation of
    validated security knowledge.


The final objective is:

    Accurate Findings
        +
    Reliable Evidence
        +
    Consistent Risk Classification
        +
    Actionable Remediation
        +
    Transparent Limitations
        +
    Reproducible Results


The framework must always prefer:

    Accuracy over volume

    Evidence over assumption

    Validation over automation

    Clarity over complexity

    Actionable remediation over generic advice


The report is the final interface between:

    The Security Engineering System

and:

    The Human Engineering / Security Team.


Therefore:

    If it was not tested,
    do not claim it was tested.

    If it was not observed,
    do not claim it occurred.

    If it was not validated,
    do not call it a confirmed vulnerability.

    If evidence is incomplete,
    state the limitation.

    If a finding is confirmed,
    provide enough information to fix it.

    If a fix is claimed,
    verify it.

    If uncertainty rema