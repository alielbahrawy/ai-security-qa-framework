# Security & QA Workflow

## 1. Purpose

This document defines the authoritative execution workflow for the
AI Security & QA Engineering Framework.

The workflow determines:

    WHEN a task should happen
    IN WHAT ORDER it should happen
    WHICH stage owns the task
    WHEN execution may happen in parallel
    WHEN execution must be sequential
    WHEN findings must be validated
    WHEN reporting is allowed
    WHEN retesting is required


This file answers:

    "When and in what order?"

The file:

    .claude/rules/tool-selection.md

answers:

    "Which tool should be used?"


The file:

    .claude/rules/severity-model.md

answers:

    "How should validated risk be classified?"


The framework therefore follows:

    CLAUDE.md
        |
        v
    Security Orchestrator
        |
        +----------------------------+
        |                            |
        v                            v
    Workflow                  Tool Selection
        |                            |
        +-------------+--------------+
                      |
                      v
                Agent Execution
                      |
                      v
                  Evidence
                      |
                      v
            Vulnerability Analyst
                      |
                      v
               Severity Model
                      |
                      v
              Report Generator


# 2. Core Principle

The framework is an orchestration system,
not a collection of independent scanners.

The workflow must transform:

    User Request

into:

    Defined Scope
        |
        v
    Execution Plan
        |
        v
    Evidence
        |
        v
    Validated Findings
        |
        v
    Risk Classification
        |
        v
    Professional Report
        |
        v
    Retest


The final objective is:

    Reliable Security & QA Results


not:

    Maximum Number of Tool Executions


# 3. Workflow Authority

The:

    Security Orchestrator

owns workflow coordination.

The Orchestrator does not personally perform
every technical operation.

Instead it coordinates:

    Security Agent
    Code Review Agent
    QA Agent
    Vulnerability Analyst
    Report Generator


The architecture is:

    User
      |
      v
    Security Orchestrator
      |
      +--> Security Agent
      |
      +--> Code Review Agent
      |
      +--> QA Agent
      |
      +--> Vulnerability Analyst
      |
      +--> Report Generator


# 4. Global Workflow

The default workflow is:

    REQUEST
       |
       v
    SCOPE
       |
       v
    AUTHORIZATION
       |
       v
    TARGET ANALYSIS
       |
       v
    OBJECTIVE DEFINITION
       |
       v
    PLAN
       |
       v
    TOOL SELECTION
       |
       v
    EXECUTION
       |
       v
    EVIDENCE COLLECTION
       |
       v
    TRIAGE
       |
       v
    VALIDATION
       |
       v
    CORRELATION
       |
       v
    SEVERITY
       |
       v
    REPORT
       |
       v
    RETEST
       |
       v
    CLOSE


# 5. Phase 0 — Receive Request

The Orchestrator receives:

    User Request


The request may involve:

    Security Audit
    Penetration Testing
    Code Review
    Vulnerability Assessment
    QA Testing
    Regression Testing
    API Testing
    E2E Testing
    Full Application Assessment


The Orchestrator must determine
what the user is actually asking for.


# 6. Request Normalization

Before execution, normalize the request into:

    Objective
    Target
    Scope
    Environment
    Constraints
    Required Output
    Available Tools
    Available Agents


Example:

    User:
        "Test my web application for security issues."


Normalize into:

    Objective:
        Security Assessment

    Target:
        Web Application

    Environment:
        Determine

    Scope:
        Determine

    Output:
        Security Findings + Report


# 7. Phase 1 — Scope Definition

No active security testing begins
until scope is understood.

Determine:

    Target
    Domains
    URLs
    APIs
    Repositories
    Applications
    Hosts
    Environments
    Accounts
    Testing Boundaries


Scope must be explicit enough
to prevent accidental expansion.


# 8. Scope Boundaries

Define:

    IN_SCOPE

and:

    OUT_OF_SCOPE


Example:

    IN_SCOPE:
        https://app.example.test
        /api/*
        Test Accounts


    OUT_OF_SCOPE:
        Production Database
        Third-Party Services
        Payment Provider


The framework must never infer
authorization to test out-of-scope assets.


# 9. Scope Expansion

If a tool discovers a new asset:

    Discovered Asset
        |
        v
    Check Scope
        |
        +--> In Scope
        |       |
        |       v
        |    Continue
        |
        +--> Unknown
        |       |
        |       v
        |    Stop / Request Authorization
        |
        +--> Out of Scope
                |
                v
              Do Not Test


Discovery does not create authorization.


# 10. Phase 2 — Authorization Check

Before active security testing,
verify that the requested activity
is authorized.

Determine:

    Is the target owned or authorized?
    Is testing explicitly permitted?
    Is the environment appropriate?
    Are credentials authorized?
    Are destructive actions permitted?


If authorization is unclear:

    Do Not Perform Active Testing


The framework may continue with
safe non-invasive analysis when appropriate.


# 11. Phase 3 — Environment Identification

Determine:

    Development
    Staging
    Production
    Local
    Containerized
    Cloud
    External
    Internal


Environment affects:

    Testing Intensity
    Safety Constraints
    Tool Selection
    Parallelization
    Execution Strategy


# 12. Production Rule

Production environments require
additional caution.

Default approach:

    Low Impact
    Controlled
    Rate Limited
    Non-Destructive
    Scope Restricted


Avoid:

    Destructive Operations
    Data Deletion
    Denial of Service
    Account Lockout
    High-Volume Testing
    Uncontrolled Exploitation


unless explicitly authorized.


# 13. Phase 4 — Target Analysis

Identify application components.

Possible layers:

    Frontend
    Backend
    API
    Database
    Authentication
    Authorization
    File Upload
    Storage
    Third-Party Integrations
    Infrastructure


The goal is to understand:

    What exists?

before deciding:

    What to test?


# 14. Source Availability

Determine whether source code is available.

Possible states:

    SOURCE_AVAILABLE
    SOURCE_PARTIAL
    SOURCE_UNAVAILABLE


If source is available:

    Code Review Agent


may perform static analysis.

If unavailable:

    Focus on runtime and functional testing.


# 15. Runtime Availability

Determine whether a live target exists.

Possible states:

    RUNTIME_AVAILABLE
    RUNTIME_PARTIAL
    RUNTIME_UNAVAILABLE


If runtime is available:

    Dynamic Security
    QA
    API Testing


may be appropriate.


# 16. Test Data

Determine:

    Test Accounts
    Test Credentials
    Test Data
    Seeded Records
    API Keys
    Environment Variables


Never expose secrets unnecessarily
in reports or logs.


# 17. Phase 5 — Objective Definition

Define what must be proven.

Examples:

    Identify vulnerabilities
    Validate authentication
    Test authorization
    Validate API security
    Test application workflows
    Find source-level weaknesses
    Verify remediation


The objective determines:

    Workflow
    Agents
    Tools
    Evidence Requirements


# 18. Phase 6 — Test Planning

The Security Orchestrator creates
an execution plan.

The plan should contain:

    Objective
    Scope
    Target
    Environment
    Agents
    Tools
    Test Categories
    Dependencies
    Safety Constraints
    Expected Evidence
    Output


Conceptual structure:

    Assessment Plan
        |
        +--> Scope
        +--> Objectives
        +--> Security Tests
        +--> Static Analysis
        +--> QA Tests
        +--> Validation
        +--> Reporting


# 19. Tool Selection

Tool selection follows:

    .claude/rules/tool-selection.md


The Orchestrator should not randomly
invoke all available tools.

General mapping:

    Source Security
        ->
    Semgrep


    Dynamic Security
        ->
    pentest-ai / PentesterFlow


    Functional / E2E
        ->
    TestSprite


Tool selection must be justified.


# 20. Agent Selection

After determining the required work:

    Security Work
        ->
    Security Agent


    Source Review
        ->
    Code Review Agent


    QA / Functional
        ->
    QA Agent


    Validation
        ->
    Vulnerability Analyst


    Reporting
        ->
    Report Generator


The Orchestrator coordinates these agents.


# 21. Execution Strategy

The workflow supports:

    Sequential Execution
    Parallel Execution


The decision depends on:

    Dependencies
    Safety
    Shared State
    Environment
    Evidence Requirements


# 22. Parallel Execution

Independent tasks may execute in parallel.

Example:

    Semgrep
        ||
    TestSprite
        ||
    Recon


when:

    They do not interfere
    They do not depend on each other
    Environment allows it


This reduces unnecessary execution time.


# 23. Sequential Execution

Tasks must execute sequentially
when one depends on another.

Example:

    Recon
       |
       v
    Endpoint Discovery
       |
       v
    Security Testing
       |
       v
    Validation


Another example:

    Code Finding
       |
       v
    Runtime Reproduction
       |
       v
    Analyst Validation


# 24. Dependency Graph

The Orchestrator should conceptually
build a dependency graph:

    Scope
      |
      v
    Authorization
      |
      v
    Planning
      |
      +-------------------+
      |                   |
      v                   v
    Static              QA
      |                   |
      v                   v
    Candidates         Behavior
      |                   |
      +---------+---------+
                |
                v
             Dynamic
                |
                v
            Validation
                |
                v
              Risk
                |
                v
             Report


# 25. Phase 7 — Static Analysis

When source code is available
and static analysis is relevant:

    Code Review Agent
        |
        v
    Semgrep
        |
        v
    Findings
        |
        v
    Triage


Static analysis identifies:

    Potential Vulnerabilities
    Dangerous Patterns
    Data Flows
    Missing Controls


It does not automatically prove exploitability.


# 26. Phase 8 — QA Testing

When functional testing is relevant:

    QA Agent
        |
        v
    TestSprite
        |
        v
    Test Results
        |
        v
    Triage


QA may validate:

    Authentication Workflow
    Authorization Workflow
    API Behavior
    Frontend Behavior
    Integration
    E2E
    Regression


# 27. Phase 9 — Dynamic Security Testing

When runtime security testing
is relevant:

    Security Agent
        |
        v
    pentest-ai / PentesterFlow
        |
        v
    Security Evidence
        |
        v
    Triage


Dynamic testing may validate:

    Authentication
    Authorization
    Input Validation
    Injection
    Access Control
    API Security
    Runtime Configuration


# 28. Evidence Collection

Every execution should produce
structured evidence where possible.

Evidence may include:

    Tool Output
    Request
    Response
    Endpoint
    Source Location
    Reproduction Steps
    Screenshots
    Logs
    Stack Traces
    Test Results
    Runtime Behavior


Evidence must remain connected
to the finding that generated it.


# 29. Evidence Integrity

Never modify evidence in a way
that changes its meaning.

Maintain:

    Source
    Timestamp
    Target
    Test
    Context


where practical.


# 30. Phase 10 — Triage

All tool outputs enter triage.

The Orchestrator or responsible Agent
should determine:

    Relevant
    Irrelevant
    Duplicate
    Candidate
    Likely False Positive
    Requires Validation


Triage reduces noise before
deep validation.


# 31. Deduplication

Multiple tools may identify
the same vulnerability.

Example:

    Semgrep
        |
        v
    SQL Injection Candidate


    pentest-ai
        |
        v
    SQL Injection Candidate


These should become:

    One Correlated Finding


with:

    Multiple Evidence Sources


# 32. Cross-Tool Correlation

Correlation is handled by:

    Vulnerability Analyst


The Analyst combines:

    Static Evidence
    Dynamic Evidence
    QA Evidence
    Application Context


to determine whether
multiple alerts represent one issue.


# 33. Phase 11 — Validation

Candidate findings must be validated.

The validation process is:

    Candidate
        |
        v
    Context Review
        |
        v
    Reproduction
        |
        v
    Impact Analysis
        |
        v
    Exploitability Analysis
        |
        v
    Scope Analysis
        |
        v
    Final Validation


# 34. Validation Ownership

The:

    Vulnerability Analyst

owns final finding validation.

Agents may provide evidence,
but they do not independently finalize
the security conclusion.


# 35. Validation Levels

Use the evidence model defined in:

    .claude/rules/severity-model.md


Conceptually:

    Tool Alert
        |
        v
    Contextual Evidence
        |
        v
    Reproduced Behavior
        |
        v
    Confirmed Exploitation
        |
        v
    Cross-Validated Evidence


# 36. False Positive Handling

If validation fails:

    Candidate
        |
        v
    FALSE_POSITIVE


Do not report it
as a vulnerability.


Record the reason when possible.


# 37. Duplicate Handling

If two findings represent
the same root cause:

    Merge


Preserve:

    All Relevant Evidence
    All Affected Components
    All Tool Sources


Do not report duplicates as separate
vulnerabilities unless their impacts
are meaningfully different.


# 38. Phase 12 — Severity Classification

After validation:

    Vulnerability Analyst
        |
        v
    Severity Model


Use:

    .claude/rules/severity-model.md


Determine:

    Severity
    Confidence
    Exploitability
    Impact
    Scope
    Business Impact


Severity must be evidence-based.


# 39. Severity Before Validation

Do not finalize severity
before sufficient validation.

A tool may provide:

    Candidate Severity


but the framework must treat it as:

    Preliminary


Final severity belongs
to the validated finding.


# 40. Phase 13 — Finding Construction

A validated finding should contain:

    Finding ID
    Title
    Severity
    Confidence
    Status
    Description
    Affected Asset
    Affected Component
    Root Cause
    Attack Vector
    Preconditions
    Evidence
    Reproduction
    Impact
    Remediation
    References
    Source Tools


# 41. Finding Status

Recommended statuses:

    DETECTED
    TRIAGED
    VALIDATING
    VALIDATED
    FALSE_POSITIVE
    DUPLICATE
    ACCEPTED_RISK
    MITIGATED
    FIXED
    REGRESSED
    INCONCLUSIVE


Status and severity are separate.


# 42. Finding Ownership

During discovery:

    Agent


During validation:

    Vulnerability Analyst


During reporting:

    Report Generator


During coordination:

    Security Orchestrator


This prevents responsibility overlap.


# 43. Phase 14 — Reporting Gate

Reporting is allowed only after
the finding reaches the required
validation state.

Default:

    Candidate
        ->
    Validate
        ->
    Severity
        ->
    Report


Unvalidated findings must be clearly
labeled if included for transparency.


# 44. Report Generation

The:

    Report Generator

consumes validated findings.

It must not invent:

    Severity
    Evidence
    Impact
    Reproduction
    Remediation


It converts structured findings
into professional reports.


# 45. Report Structure

A typical report may contain:

    Executive Summary
    Scope
    Methodology
    Environment
    Findings Summary
    Detailed Findings
    Risk Overview
    Recommendations
    Limitations
    Retest Status


# 46. Executive Summary

The executive summary should communicate:

    Overall Security Posture
    Critical Findings
    High Findings
    Major Risks
    Recommended Priorities


It should not contain unsupported claims.


# 47. Technical Findings

Each finding should clearly show:

    What happened
    Where it happened
    Why it happened
    How it can be reproduced
    What impact it causes
    How to fix it


# 48. Phase 15 — Remediation

For each validated vulnerability,
provide remediation guidance.

Remediation should target:

    Root Cause


not only:

    Symptom


Example:

    Do not only block one malicious payload.


Instead:

    Fix unsafe input handling.


# 49. Remediation Ownership

The framework may recommend fixes,
but application owners implement them.

After implementation:

    Retest


is required when appropriate.


# 50. Phase 16 — Retesting

Retesting verifies remediation.

The workflow is:

    Original Finding
        |
        v
    Fix Applied
        |
        v
    Retest
        |
        v
    Reproduce Original Condition
        |
        v
    Verify Fix
        |
        v
    Regression Check
        |
        v
    Update Finding


# 51. Retest Tools

Use the original evidence source
when possible.

Example:

    Original:
        TestSprite


    Retest:
        TestSprite


For security:

    Original:
        pentest-ai


    Retest:
        pentest-ai


For source-level:

    Original:
        Semgrep


    Retest:
        Semgrep


Equivalent validation may be used
when necessary.


# 52. Retest Outcomes

Possible outcomes:

    FIXED
    PARTIALLY_FIXED
    NOT_FIXED
    REGRESSED
    INCONCLUSIVE


The report must clearly distinguish them.


# 53. Regression Testing

A security fix must not
break expected functionality.

Therefore:

    Security Retest
        +
    QA Regression


may be required.


Example:

    Authorization Fix
        |
        +--> Security Retest
        |
        +--> TestSprite Regression


# 54. Phase 17 — Closure

A task may close when:

    Testing Completed
    Findings Validated
    Severity Assigned
    Report Generated
    Limitations Documented
    Retest Completed when required


The Orchestrator should not declare
completion prematurely.


# 55. Completion Criteria

A security assessment is complete when:

    Scope Completed
        +
    Planned Tests Executed
        +
    Relevant Findings Validated
        +
    Limitations Recorded
        +
    Report Produced


Not:

    "All tools ran."


# 56. Incomplete Assessment

If some planned tests could not execute:

    Do not claim full coverage.


Instead report:

    Testing Limitation


Example:

    "Dynamic API testing could not be completed
     because the target environment was unavailable."


# 57. Tool Failure

If a tool fails:

    Detect
        |
        v
    Diagnose
        |
        v
    Retry when safe
        |
        v
    Alternative
        |
        v
    Document


Do not silently continue
as if the test succeeded.


# 58. Authentication Failure

If credentials fail:

    Do not repeatedly brute-force
    or create unsafe authentication traffic.


Instead:

    Verify Credentials
    Verify Environment
    Check Test Account
    Document Limitation


# 59. Environment Failure

If the environment becomes unavailable:

    Pause Dependent Tests


Do not fabricate results.


Resume when:

    Environment Restored


or:

    Mark Testing Incomplete


# 60. Safety Stop Conditions

Immediately stop or pause when:

    Scope Becomes Unclear
    Authorization Becomes Unclear
    Production Impact Is Observed
    Destructive Behavior Is Triggered
    Unexpected Data Exposure Occurs
    Testing Risks Service Stability
    Tool Behavior Exceeds Intended Scope


The Orchestrator must prioritize:

    Safety
    Scope
    Evidence


# 61. Unexpected Sensitive Data

If testing reveals sensitive information:

    Stop unnecessary access
    Minimize exposure
    Record only required evidence
    Avoid copying unnecessary secrets
    Notify through the appropriate reporting path


Never include secrets unnecessarily
in final reports.


# 62. Unexpected Critical Finding

If a critical vulnerability is discovered:

    Preserve Evidence
        |
        v
    Validate Carefully
        |
        v
    Assess Immediate Risk
        |
        v
    Escalate According to Engagement Rules


Do not perform uncontrolled exploitation
just to increase evidence.


# 63. Attack Chain Workflow

For chained vulnerabilities:

    Finding A
        |
        v
    Finding B
        |
        v
    Finding C
        |
        v
    Combined Impact


The Analyst must determine:

    Individual Findings
    Chain Relationship
    Combined Impact


# 64. Attack Chain Validation

Do not assume:

    A + B = Exploitable Chain


The chain must be demonstrated
or strongly supported by evidence.


# 65. Full Assessment Workflow

For a complete application assessment:

    1. Scope
    2. Authorization
    3. Environment
    4. Application Mapping
    5. Test Planning
    6. Static Analysis
    7. QA Testing
    8. Dynamic Security Testing
    9. Triage
    10. Correlation
    11. Validation
    12. Severity
    13. Reporting
    14. Remediation
    15. Retesting
    16. Closure


# 66. Web Application Workflow

Typical sequence:

    Scope
      |
      v
    Recon / Mapping
      |
      v
    Static Analysis
      |
      v
    Functional Testing
      |
      v
    Dynamic Security Testing
      |
      v
    Correlation
      |
      v
    Validation
      |
      v
    Reporting


# 67. API Workflow

Typical sequence:

    API Scope
      |
      v
    Endpoint Discovery
      |
      v
    Functional API Tests
      |
      v
    Authentication Tests
      |
      v
    Authorization Tests
      |
      v
    Input Validation
      |
      v
    Dynamic Security
      |
      v
    Correlation
      |
      v
    Validation
      |
      v
    Reporting


# 68. Source Code Workflow

Typical sequence:

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
    Context Analysis
       |
       v
    Runtime Validation when available
       |
       v
    Vulnerability Analyst
       |
       v
    Report


# 69. QA Workflow

Typical sequence:

    Requirements / PRD
       |
       v
    QA Agent
       |
       v
    TestSprite
       |
       v
    Test Execution
       |
       v
    Results
       |
       v
    Regression / Retest


Security-relevant failures
may be escalated to:

    Vulnerability Analyst


# 70. Security Workflow

Typical sequence:

    Target
       |
       v
    Security Agent
       |
       v
    Recon
       |
       v
    Dynamic Testing
       |
       v
    Findings
       |
       v
    Validation
       |
       v
    Vulnerability Analyst


# 71. Cross-Agent Handoff

Agents must hand off structured information.

Example:

    Security Agent
        |
        v
    Candidate Finding
        |
        v
    Vulnerability Analyst


The handoff should include:

    Finding
    Target
    Evidence
    Reproduction
    Tool
    Context
    Limitations


# 72. Agent Handoff Rules

Do not hand off:

    Unexplained Raw Output


Prefer:

    Structured Finding


The receiving Agent should know:

    What was found
    Where it was found
    Why it matters
    What remains to be validated


# 73. Shared Intelligence

The framework's agents should share
relevant intelligence.

The conceptual layer is:

    Discovery
        |
        v
    Shared Evidence
        |
        +----------------+
        |                |
        v                v
    Security           QA
        |                |
        +--------+-------+
                 |
                 v
        Vulnerability Analyst
                 |
                 v
          Final Findings


Shared intelligence must not
cause uncontrolled scope expansion.


# 74. Knowledge Integration

The workflow may consult:

    .claude/knowledge/security-patterns.md
    .claude/knowledge/common-vulnerabilities.md
    .claude/knowledge/testing-strategy.md


Knowledge supports:

    Planning
    Detection
    Validation
    Interpretation


Knowledge does not replace
actual evidence.


# 75. Skill Integration

The workflow should invoke
the appropriate skill for each domain.

Security:

    .claude/skills/security-audit/SKILL.md


Code Review:

    .claude/skills/code-review/SKILL.md


QA:

    .claude/skills/qa-testing/SKILL.md


Reporting:

    .claude/skills/reporting/SKILL.md


Skills define:

    HOW the domain task should be performed.


Rules define:

    WHAT constraints and workflow govern it.


# 76. Rule Interaction

The three primary rules work together:

    tool-selection.md
        |
        v
    WHICH TOOL


    workflow.md
        |
        v
    WHEN TO EXECUTE


    severity-model.md
        |
        v
    HOW TO CLASSIFY RISK


Together:

    Scope
      |
      v
    Workflow
      |
      v
    Tool Selection
      |
      v
    Execution
      |
      v
    Validation
      |
      v
    Severity
      |
      v
    Reporting


# 77. Orchestrator Control Loop

The Security Orchestrator should
operate conceptually as:

    RECEIVE
       |
       v
    UNDERSTAND
       |
       v
    SCOPE
       |
       v
    PLAN
       |
       v
    DELEGATE
       |
       v
    MONITOR
       |
       v
    COLLECT
       |
       v
    CORRELATE
       |
       v
    VALIDATE
       |
       v
    CLASSIFY
       |
       v
    REPORT
       |
       v
    RETEST
       |
       v
    CLOSE

# 78A. State Persistence and Resume

The framework must maintain persistent execution state
throughout the assessment lifecycle.

The state layer is controlled by:

    Security Orchestrator

State persistence exists to ensure that
an interrupted assessment can resume
from the latest valid checkpoint
instead of restarting from the beginning.

The state layer is responsible for:

    Current assessment
    Current phase
    Current task
    Completed tasks
    Pending tasks
    Running tasks
    Failed tasks
    Blocked tasks
    Findings
    Validation status
    Tool execution status
    Agent handoffs
    Checkpoints
    Resume information

The state architecture is:

    Security Orchestrator
            |
            v
       Load State
            |
            v
     Determine Action
            |
            v
       Execute Task
            |
            v
      Collect Result
            |
            v
     Validate Result
            |
            v
    Write Checkpoint
            |
            v
        Save State
            |
            v
     Continue Workflow


# 78B. State Authority

The Security Orchestrator is the
single workflow authority for state.

Agents may produce:

    Results
    Findings
    Evidence
    Status Updates

but they must not independently
change the overall assessment state.

The Orchestrator decides:

    What has completed
    What remains pending
    What must be retried
    What must be resumed
    What must be validated
    What stage comes next


# 78C. Persistent State Location

Persistent workflow state is stored under:

    .claude/state/

The state directory contains:

    README.md
    Checkpoints
    Assessment State
    Resume Information

The exact state structure is defined by
the state schema and supporting files
under:

    .claude/state/

State files must contain machine-readable
information whenever possible.


# 78D. Checkpoint Model

The Orchestrator must create checkpoints
at meaningful workflow boundaries.

A checkpoint should be created after:

    Scope completion
    Authorization confirmation
    Planning completion
    Agent completion
    Tool execution
    Evidence collection
    Finding validation
    Severity classification
    Report generation
    Retesting

A checkpoint represents the latest
known valid workflow state.

The framework must never assume that
an interrupted task completed successfully.


# 78E. Resume Behavior

When Claude Code starts or an assessment
is resumed, the Security Orchestrator
must first determine whether persistent
state exists.

The flow is:

    Start
      |
      v
    Load State
      |
      +----------------------+
      |                      |
      v                      v
    No State              State Exists
      |                      |
      v                      v
    Initialize          Validate State
                             |
                             v
                       Determine Status
                             |
                 +-----------+-----------+
                 |           |           |
                 v           v           v
              Resume      Retry       Reconcile
                 |           |           |
                 +-----------+-----------+
                             |
                             v
                     Continue Workflow


# 78F. Resume Decision Rules

If the previous task is:

    COMPLETED

continue with the next pending task.

If the previous task is:

    FAILED

determine whether it is safe
and useful to retry.

If the previous task is:

    BLOCKED

determine whether the blocking
condition has been resolved.

If the previous task is:

    RUNNING

do not assume completion.

The Orchestrator must reconcile
the task against its last checkpoint
and determine whether it should:

    Resume
    Retry
    Revalidate
    Restart the interrupted task


# 78G. Interrupted Execution

An interruption may occur because of:

    Internet failure
    Laptop shutdown
    Claude Code termination
    Terminal closure
    Process failure
    MCP failure
    Tool failure
    System restart
    Authentication failure

An interruption must not cause
the entire assessment to restart
unless the persisted state is invalid
or the workflow explicitly requires it.


# 78H. Safe Resume Principle

Resume from the latest valid checkpoint,
not from the beginning.

Example:

    Scope
      |
      v
    Planning
      |
      v
    Semgrep
      |
      v
    TestSprite
      |
      X
    INTERRUPTION

After restart:

    Load State
      |
      v
    Verify Semgrep
      |
      v
    Verify TestSprite Status
      |
      v
    Resume Pending Work
      |
      v
    Continue Validation


# 78I. Idempotency

The Orchestrator should avoid
duplicating completed work.

Before executing a task,
check whether the state already records:

    COMPLETED

If completed and the result remains valid:

    Do Not Repeat

If the result is incomplete,
invalid, expired, or corrupted:

    Re-execute

This prevents unnecessary:

    Tool Calls
    Tests
    Network Requests
    Duplicate Findings


# 78J. State Consistency

Before resuming, validate:

    Assessment ID
    Target
    Scope
    Environment
    Workflow Phase
    Task Status
    Tool Status
    Finding Status
    Checkpoint Integrity

If state is inconsistent:

    Do Not Blindly Resume

Instead:

    Detect Conflict
        |
        v
    Reconcile State
        |
        v
    Select Safe Recovery Point
        |
        v
    Continue


# 78K. State and Evidence Relationship

State must reference
the evidence generated by execution.

Conceptually:

    Task
      |
      v
    Execution
      |
      v
    Evidence
      |
      v
    Finding
      |
      v
    Validation

State must not claim:

    COMPLETED

when the corresponding evidence
does not exist or execution status
cannot be verified.


# 78L. State and Findings

Findings must persist independently
from transient agent conversations.

A finding may move through:

    DETECTED
        |
        v
    TRIAGED
        |
        v
    VALIDATING
        |
        v
    VALIDATED
        |
        +--> FALSE_POSITIVE
        |
        +--> DUPLICATE
        |
        v
    REPORTED
        |
        v
    RETESTING
        |
        v
    FIXED / NOT_FIXED / REGRESSED


# 78M. State and Parallel Execution

When multiple tasks execute in parallel,
each task must maintain its own status.

Example:

    Semgrep
       |
       +--> COMPLETED

    TestSprite
       |
       +--> RUNNING

    Dynamic Security
       |
       +--> BLOCKED

The Orchestrator must preserve
these states independently.

After interruption:

    COMPLETED
        -> Do Not Repeat

    RUNNING
        -> Reconcile

    BLOCKED
        -> Check Dependency

This prevents restarting
all parallel branches unnecessarily.


# 78N. State Transitions

Assessment state should follow
controlled transitions.

Example:

    INITIALIZING
        |
        v
    SCOPING
        |
        v
    PLANNING
        |
        v
    EXECUTING
        |
        v
    TRIAGING
        |
        v
    VALIDATING
        |
        v
    REPORTING
        |
        v
    RETESTING
        |
        v
    COMPLETED

Exceptional states include:

    BLOCKED
    FAILED
    CANCELLED
    RECOVERY_REQUIRED


# 78O. Recovery

If state cannot be safely resumed:

    Load State
        |
        v
    Detect Invalid State
        |
        v
    Identify Last Valid Checkpoint
        |
        v
    Restore From Checkpoint
        |
        v
    Reconcile Pending Tasks
        |
        v
    Continue Workflow

Never fabricate missing state.

Never mark an unverified task
as completed during recovery.


# 78P. Resume Transparency

When resuming an interrupted assessment,
the Orchestrator should know:

    What was completed?
    What was interrupted?
    What failed?
    What remains?
    What must be retried?
    What evidence already exists?
    What findings already exist?

The user should not need to manually
reconstruct the entire previous workflow.


# 78Q. State Lifecycle

The complete lifecycle is:

    CREATE
      |
      v
    INITIALIZE
      |
      v
    EXECUTE
      |
      v
    CHECKPOINT
      |
      v
    SAVE
      |
      v
    RESUME
      |
      v
    RECONCILE
      |
      v
    CONTINUE
      |
      v
    COMPLETE
      |
      v
    ARCHIVE


# 78R. Final Resume Principle

The framework must behave as:

    PLAN
      ->
    EXECUTE
      ->
    CHECKPOINT
      ->
    SAVE
      ->
    INTERRUPT
      ->
    RESUME
      ->
    RECONCILE
      ->
    CONTINUE
      ->
    VALIDATE
      ->
    REPORT
      ->
    RETEST
      ->
    CLOSE

The goal is:

    "Continue from the last valid state."

Not:

    "Start the entire assessment again."

State persistence is therefore a
core part of workflow correctness,
reliability, reproducibility,
and assessment continuity.

# 78. Do Not Skip Stages

The Orchestrator must not skip:

    Scope
    Authorization
    Validation


because:

    Speed is not more important than correctness.


Other stages may be combined
when the task is small,
but the underlying responsibilities
must remain covered.


# 79. Small Task Workflow

For a simple task:

    Request
      |
      v
    Scope
      |
      v
    Appropriate Agent
      |
      v
    Tool
      |
      v
    Result
      |
      v
    Validation
      |
      v
    Output


The full framework does not need
to become unnecessarily complex.


# 80. Large Task Workflow

For a complex engagement:

    Request
      |
      v
    Scope
      |
      v
    Authorization
      |
      v
    Architecture Analysis
      |
      v
    Planning
      |
      +-----------------------------+
      |             |               |
      v             v               v
    Static         QA            Security
      |             |               |
      +-------------+---------------+
                    |
                    v
                Correlation
                    |
                    v
                Validation
                    |
                    v
                 Severity
                    |
                    v
                 Report
                    |
                    v
                 Retest


# 81. Evidence-Driven Iteration

The workflow may iterate.

Example:

    Initial Test
        |
        v
    Finding
        |
        v
    New Evidence Needed
        |
        v
    Additional Test
        |
        v
    Validation


The Orchestrator may request
additional testing when evidence
is insufficient.


# 82. Adaptive Testing

The framework should adapt
based on findings.

Example:

    Initial Discovery
        |
        v
    Authorization Weakness
        |
        v
    Expand Validation Within Scope
        |
        v
    Test Related Endpoints


This is:

    Controlled Adaptive Testing


not:

    Uncontrolled Scope Expansion.


# 83. Validation Depth

Validation depth should depend on:

    Severity Candidate
    Exploitability
    Impact
    Evidence Quality
    Scope
    Environment


Higher-risk candidates
generally require deeper validation.


# 84. Time Constraints

When time is limited:

    Prioritize:

    Critical Candidate
    High Candidate
    Authentication
    Authorization
    Sensitive Data
    Remote Code Execution
    Major Business Logic


Do not sacrifice:

    Evidence Integrity


for speed.


# 85. Coverage Tracking

The Orchestrator should track:

    Planned Tests
    Executed Tests
    Skipped Tests
    Failed Tests
    Blocked Tests
    Validated Findings


This provides honest coverage reporting.


# 86. Test Status

Recommended test statuses:

    PLANNED
    READY
    RUNNING
    COMPLETED
    FAILED
    BLOCKED
    SKIPPED
    CANCELLED


A skipped test must not
be represented as:

    PASSED


# 87. Assessment Status

Recommended assessment states:

    INITIALIZING
    SCOPING
    PLANNING
    EXECUTING
    TRIAGING
    VALIDATING
    REPORTING
    RETESTING
    COMPLETED
    BLOCKED
    CANCELLED


# 88. Completion Integrity

The Orchestrator must be able
to explain:

    What was tested?
    What was not tested?
    What failed?
    What was validated?
    What remains uncertain?


If these questions cannot be answered,
the assessment is not fully complete.


# 89. Audit Trail

Where practical, maintain:

    Tool
    Agent
    Target
    Action
    Result
    Timestamp
    Status


This improves:

    Reproducibility
    Debugging
    Reporting
    Retesting


# 90. Reproducibility

A finding should be reproducible
whenever practical.

The report should contain enough
information for an authorized tester
to understand:

    How it was discovered
    How it was validated
    What conditions were required


# 91. Security vs QA Separation

The framework must distinguish:

    Functional Failure


from:

    Security Vulnerability


Example:

    Login button crashes


is primarily:

    QA


Example:

    Login can be bypassed


is:

    Security


A functional defect may become
security-relevant after analysis.


# 92. Security Escalation from QA

If TestSprite identifies behavior
that appears security-sensitive:

    QA Agent
        |
        v
    Security Escalation
        |
        v
    Security Agent
        |
        v
    Vulnerability Analyst


QA should not independently
declare a security vulnerability.


# 93. Security Escalation from Static Analysis

If Semgrep identifies a potentially
high-impact issue:

    Code Review Agent
        |
        v
    Security Agent
        |
        v
    Dynamic Validation
        |
        v
    Vulnerability Analyst


when runtime validation is possible.


# 94. Security Escalation from Dynamic Testing

If dynamic testing finds a vulnerability:

    Security Agent
        |
        v
    Vulnerability Analyst


If source code is available,
the Analyst may request:

    Code Review Agent


to determine root cause.


# 95. Root Cause Analysis

Root cause should be investigated
after meaningful evidence exists.

Example:

    Dynamic Finding
        |
        v
    Reproduction
        |
        v
    Source Review
        |
        v
    Root Cause


Root cause improves:

    Remediation Quality


# 96. Remediation Validation

A fix is not considered effective
merely because code changed.

Validation should verify:

    Vulnerability No Longer Exploitable
        +
    Intended Functionality Still Works


Therefore:

    Security Retest
        +
    QA Regression


may both be required.


# 97. Report Finalization

Before final report:

    Validate Findings
    Confirm Severity
    Confirm Evidence
    Confirm Reproduction
    Confirm Remediation
    Document Limitations
    Remove Unnecessary Secrets
    Verify Scope


Then:

    Report Generator


produces the final report.


# 98. Final Report Gate

The final report must not contain:

    Fabricated Results
    Unvalidated Critical Claims
    Unexplained Tool Alerts
    Hidden Testing Limitations
    Out-of-Scope Findings Presented as Tested


# 99. Final Assessment Gate

Before declaring completion:

    Scope Complete?
        |
        +--> NO -> Document limitation


    Planned Tests Complete?
        |
        +--> NO -> Document limitation


    Findings Validated?
        |
        +--> NO -> Continue validation


    Severity Assigned?
        |
        +--> NO -> Analyst review


    Report Complete?
        |
        +--> NO -> Report Generator


    Retest Required?
        |
        +--> YES -> Retest


    Then:

        COMPLETED


# 100. Final Architecture

The complete framework workflow is:

    Claude Code
        |
        v
    Security Orchestrator
        |
        v
    Scope + Authorization
        |
        v
    Planning
        |
        +----------------------+----------------------+
        |                      |                      |
        v                      v                      v
    Code Review            Security                QA
        |                      |                      |
        v                      v                      v
    Semgrep              pentest-ai             TestSprite
                             /
                       PentesterFlow
        |                      |                      |
        +----------------------+----------------------+
                               |
                               v
                         Shared Evidence
                               |
                               v
                    Vulnerability Analyst
                               |
                               v
                         Validation
                               |
                               v
                        Severity Model
                               |
                               v
                       Report Generator
                               |
                               v
                          Remediation
                               |
                               v
                    Security Retest + QA
                               |
                               v
                            Closure


# 101. Final Principle

The framework must operate as:

    PLAN
      ->
    EXECUTE
      ->
    COLLECT
      ->
    CORRELATE
      ->
    VALIDATE
      ->
    CLASSIFY
      ->
    REPORT
      ->
    RETEST


The goal is not:

    "Run every tool."


The goal is:

    "Produce reliable, reproducible,
     evidence-backed security and QA results."


Final rule:

    Scope controls boundaries.

    Authorization controls permission.

    Workflow controls order.

    Tool Selection controls tool choice.

    Agents perform specialized work.

    Evidence connects execution to findings.

    Vulnerability Analyst validates findings.

    Severity Model classifies validated risk.

    Report Generator communicates results.

    Retesting verifies remediation.

    The Security Orchestrator coordinates
    the entire lifecycle without replacing
    the specialized responsibilities of the agents.