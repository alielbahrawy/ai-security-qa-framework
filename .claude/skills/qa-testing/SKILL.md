---
name: qa-testing
description: Execute structured functional, API, integration, regression, and end-to-end quality testing using the QA Agent and TestSprite, while correlating security-relevant failures with the security validation pipeline.
---

# QA Testing Skill

## 1. Purpose

This skill defines the operational QA and testing workflow
within the AI Security & QA Engineering Framework.

Its purpose is to transform an application under test into:

    Application
        |
        v
    Test Understanding
        |
        v
    Test Strategy
        |
        v
    Test Plan
        |
        v
    Test Execution
        |
        v
    Failure Analysis
        |
        v
    Security Correlation
        |
        v
    Validated Results
        |
        v
    Reporting


The skill covers:

    Functional Testing
    API Testing
    Integration Testing
    End-to-End Testing
    Regression Testing
    Negative Testing
    Boundary Testing
    Workflow Testing
    Security-Relevant Functional Testing


The primary execution platform is:

    TestSprite


when appropriate and available.


# 2. Framework Position

The QA layer operates as:

    Claude Code
         |
         v
    Security Orchestrator
         |
         v
    QA Agent
         |
         v
    QA Testing Skill
         |
         v
      TestSprite
         |
         v
    Test Results
         |
    +----+----------------------+
    |                           |
    v                           v
Functional Defects       Security-Relevant
                              Behavior
    |                           |
    v                           v
QA Analysis             Vulnerability Analyst
    |                           |
    +-------------+-------------+
                  |
                  v
          Report Generator


The Security Orchestrator decides:

    When QA testing is required.

The QA Agent decides:

    How QA testing should be executed.

This skill defines:

    How the testing process should operate.


# 3. Relationship With Security Testing

QA testing and security testing are complementary.

QA asks:

    "Does the application behave correctly?"


Security testing asks:

    "Can the application's security boundaries
     be bypassed or abused?"


The same test may provide evidence for both.


Example:

    QA:
        User requests profile.

    Expected:
        Own profile returned.

    Actual:
        Another user's profile returned.


This is:

    Functional Failure

and potentially:

    Security Vulnerability


Therefore:

    QA Failure
        |
        v
    Security-Relevant?
        |
       Yes
        |
        v
    Vulnerability Analyst


# 4. Required Framework Context

Before executing QA testing, use:

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


Do not contradict framework-wide rules.


# 5. Authorization and Scope

Before executing tests, determine:

    Application
    Environment
    Test Accounts
    APIs
    URLs
    User Roles
    Test Data
    Testing Restrictions


Confirm:

    In-Scope Targets


and:

    Out-of-Scope Targets


Never expand testing scope automatically.


# 6. Environment Classification

Identify:

    Development
    Test
    Staging
    Production


Preferred order:

    Test
        >
    Staging
        >
    Development
        >
    Production


Production testing requires appropriate authorization
and additional caution.


# 7. Test Modes

Supported modes include:

    Full Application QA
    Frontend Testing
    Backend Testing
    API Testing
    End-to-End Testing
    Regression Testing
    Smoke Testing
    Integration Testing
    Negative Testing
    Workflow Testing
    Targeted Test


The Orchestrator determines the required mode.


# 8. TestSprite Role

TestSprite is the primary QA MCP integration.

Its role may include:

    Project Bootstrap
    Codebase Understanding
    Test Plan Generation
    Frontend Test Planning
    Backend Test Planning
    Test Execution
    Result Collection
    Result Inspection
    Regression Support


TestSprite is a QA tool.

Do not treat TestSprite as:

    The final security authority.


Security conclusions remain with:

    Vulnerability Analyst


# 9. Test Lifecycle

Use:

    01. Scope
          |
          v
    02. Application Understanding
          |
          v
    03. Test Strategy
          |
          v
    04. Test Plan
          |
          v
    05. Environment Preparation
          |
          v
    06. Test Execution
          |
          v
    07. Failure Analysis
          |
          v
    08. Security Correlation
          |
          v
    09. Regression
          |
          v
    10. Reporting


Not every engagement requires every phase.


# 10. Phase 1 — Scope

Define:

    Application
    Features
    APIs
    User Roles
    Workflows
    Environment
    Browser / Client
    Test Data


Also record:

    Known Limitations
    Out-of-Scope Features
    Disabled Integrations


# 11. Phase 2 — Application Understanding

Understand:

    Application Architecture
    User Journeys
    Core Features
    APIs
    Authentication
    Authorization
    External Integrations
    Data Dependencies


Map:

    User
        |
        v
    Frontend
        |
        v
    API
        |
        v
    Backend
        |
        v
    Database


# 12. User Journey Mapping

Identify critical workflows such as:

    Registration
        |
        v
    Login
        |
        v
    Dashboard
        |
        v
    Create Resource
        |
        v
    Edit Resource
        |
        v
    Delete Resource


Other examples:

    Checkout
    Payment
    Password Reset
    File Upload
    Search
    Messaging
    Administration


Prioritize business-critical journeys.


# 13. Test Strategy

A test strategy should define:

    What to test
    Why to test it
    How to test it
    Expected behavior
    Negative behavior
    Security-sensitive behavior
    Priority
    Dependencies


Prioritize:

    Critical Features
    Authentication
    Authorization
    Financial Operations
    Data Modification
    Administrative Functions
    Sensitive Data
    External Integrations


# 14. Test Plan

A test plan should contain:

    Test ID
    Feature
    Preconditions
    Test Data
    Steps
    Expected Result
    Actual Result
    Status
    Priority
    Evidence


Example:

    Test ID:
        QA-001

    Feature:
        User Profile

    Preconditions:
        Authenticated user exists.

    Steps:
        1. Login.
        2. Open profile.
        3. Update display name.

    Expected:
        Profile updates successfully.

    Status:
        PASS


# 15. Test Case Design

Each test should define:

    Preconditions
    Action
    Expected Result
    Observed Result


Avoid ambiguous tests.

Bad:

    "Test login."


Good:

    "Submit valid credentials and verify that the
     authenticated user reaches the dashboard."


# 16. Positive Testing

Verify expected valid behavior.

Examples:

    Valid Login
    Valid Registration
    Valid API Request
    Valid Resource Creation
    Valid Update
    Valid Delete


Positive tests establish:

    Baseline Application Behavior


# 17. Negative Testing

Test invalid or unexpected inputs.

Examples:

    Invalid Credentials
    Missing Required Fields
    Invalid IDs
    Invalid Formats
    Expired Tokens
    Unauthorized Actions
    Unsupported Methods
    Invalid State Transitions


Expected behavior should be:

    Controlled Failure


not:

    Application Crash


# 18. Boundary Testing

Test boundaries such as:

    Minimum Value
    Maximum Value
    Empty Value
    Null
    Very Long Input
    Large File
    Large Collection
    Zero
    Negative Value


Where applicable, test:

    Off-by-One Conditions


# 19. Input Testing

Evaluate:

    Required Fields
    Optional Fields
    Data Types
    Formats
    Length
    Encoding
    Special Characters


Examples:

    Empty String
    Unicode
    Whitespace
    Long Strings
    Special Characters


The objective is:

    Correct Validation


# 20. Authentication Testing

Functional authentication tests include:

    Registration
    Login
    Logout
    Password Reset
    Password Change
    Session Expiration
    MFA
    Account Recovery


Expected behavior should be clearly defined.


# 21. Authentication Security Correlation

If QA observes:

    Login succeeds without valid credentials

or:

    Expired session remains authorized


the result must be marked:

    Security-Relevant


and passed to:

    Vulnerability Analyst


Do not independently assign final security severity.


# 22. Authorization Testing

Test authorized and unauthorized workflows.

Example:

    User A
        |
        v
    Own Resource
        |
        v
    Allowed


Then:

    User A
        |
        v
    User B Resource
        |
        v
    Should Be Denied


Potential security findings include:

    Broken Access Control
    IDOR / BOLA
    Privilege Escalation
    Tenant Isolation Failure


These require validation.


# 23. Role-Based Testing

Where roles exist, test:

    Anonymous
    User
    Moderator
    Manager
    Administrator


Do not assume role names.

Use actual application roles.


# 24. Workflow Testing

Test complete workflows:

    Start
        |
        v
    Action 1
        |
        v
    Action 2
        |
        v
    Action 3
        |
        v
    Completion


Also test:

    Missing Step
    Repeated Step
    Reordered Step
    Invalid Transition


Business logic defects often appear here.


# 25. State Transition Testing

Identify application states.

Example:

    Draft
        |
        v
    Submitted
        |
        v
    Approved
        |
        v
    Completed


Test:

    Valid Transition

and:

    Invalid Transition


Security-relevant failures include:

    Unauthorized State Change
    Approval Bypass
    Workflow Bypass


# 26. API Testing

For APIs test:

    Endpoint
    Method
    Authentication
    Authorization
    Headers
    Parameters
    Request Body
    Response
    Status Code


Verify:

    Expected Schema
    Error Schema
    Validation
    Access Control


# 27. API Contract Testing

Verify:

    Request Contract
    Response Contract
    Data Types
    Required Fields
    Optional Fields
    Status Codes


Unexpected behavior should be documented.


# 28. API Negative Testing

Test:

    Missing Parameters
    Invalid Parameters
    Invalid Types
    Malformed JSON
    Unsupported Methods
    Missing Authentication
    Invalid Authentication
    Unauthorized Resources


Expected result:

    Safe and predictable rejection.


# 29. Frontend Testing

Verify:

    Navigation
    Forms
    Validation
    Loading States
    Error States
    Empty States
    Responsive Behavior
    Authentication State
    User Feedback


Do not treat frontend behavior as the final authorization boundary.


# 30. Backend Testing

Verify:

    Business Logic
    Validation
    API Behavior
    Persistence
    Error Handling
    Authentication
    Authorization


Backend failures may be security-relevant.


# 31. Integration Testing

Test boundaries between:

    Frontend
        |
        v
    API


    API
        |
        v
    Database


    API
        |
        v
    External Service


Look for:

    Contract Mismatch
    Error Propagation
    Authentication Problems
    Data Transformation Errors


# 32. End-to-End Testing

E2E testing should represent realistic user journeys.

Example:

    Register
        |
        v
    Login
        |
        v
    Create Resource
        |
        v
    Edit Resource
        |
        v
    Verify Result


Prioritize critical workflows.


# 33. Regression Testing

Regression tests verify that:

    Existing Functionality

remains correct after:

    Code Changes
    Security Fixes
    Dependency Updates
    Configuration Changes


Security remediation should trigger targeted regression testing.


# 34. Security Fix Regression

After a security fix:

    Original Vulnerable Path
        |
        v
    Re-test
        |
        v
    Confirm Blocked
        |
        v
    Test Legitimate Behavior


The fix must not break authorized functionality unnecessarily.


# 35. Smoke Testing

Smoke tests verify that the application is fundamentally operational.

Check:

    Application Startup
    Authentication
    Core API
    Critical User Journey
    Database Connectivity


Smoke testing should occur before deep E2E testing when appropriate.


# 36. Test Data

Use controlled test data.

Prefer:

    Synthetic Users
    Synthetic Emails
    Synthetic Records
    Non-Production Credentials


Do not unnecessarily expose:

    Real Personal Data
    Production Secrets
    Real Customer Information


# 37. Account Matrix

When multiple roles exist, maintain:

    Account
    Role
    Permissions
    Test Purpose


Example:

    Account A
        Role: User

    Account B
        Role: Admin


Use separate accounts when testing authorization boundaries.


# 38. Evidence Collection

For failed tests preserve:

    Test ID
    Environment
    Preconditions
    Steps
    Expected Result
    Actual Result
    Timestamp when available
    Screenshot when applicable
    Logs when relevant
    API Request
    API Response


Redact sensitive information.


# 39. Failure Classification

Classify failures as:

    Application Defect
    Test Defect
    Environment Failure
    Dependency Failure
    Configuration Failure
    Security-Relevant Failure
    Inconclusive


Do not classify every failure as:

    Application Bug


# 40. Test Defect

A test defect occurs when:

    Test Assumption Is Wrong

or:

    Test Implementation Is Incorrect


Example:

    Incorrect Selector
    Invalid Test Data
    Wrong API Contract


Fix the test before blaming the application.


# 41. Environment Failure

Examples:

    Service Unavailable
    Database Down
    Network Failure
    Missing Environment Variable
    External Dependency Offline


Environment failures should not become application defects automatically.


# 42. Security-Relevant Failure Detection

Every significant failure should be evaluated:

    Does this affect:

        Authentication?
        Authorization?
        Confidentiality?
        Integrity?
        Availability?
        Sensitive Data?
        Trust Boundaries?


If yes:

    Security-Relevant = TRUE


Then:

    Forward to Vulnerability Analyst


# 43. QA-to-Security Handoff

Security-relevant QA evidence should contain:

    Finding Candidate ID
    Test ID
    Target
    Role
    Preconditions
    Steps
    Expected Behavior
    Actual Behavior
    Evidence
    Security Impact
    Reproduction


Example:

    Test:
        QA-AC-004

    Role:
        User

    Expected:
        Access denied.

    Actual:
        Another user's record returned.

    Security Relevance:
        Potential Broken Object-Level Authorization.


# 44. Vulnerability Analyst Handoff

The Vulnerability Analyst determines:

    Validated
    False Positive
    Duplicate
    Inconclusive
    Not Exploitable


The QA layer must not override that decision.


# 45. QA vs Security Finding

A failure such as:

    "Page displays incorrect color."


is:

    Functional Defect


A failure such as:

    "User can access another user's private data."


is:

    Security-Relevant


A failure such as:

    "Server crashes when malformed input is submitted."


may be:

    Functional

or:

    Security-Relevant


depending on:

    Impact
    Exploitability
    Context


# 46. Test Prioritization

Prioritize tests by:

    Business Criticality
    Security Sensitivity
    User Impact
    Change Risk
    Complexity
    Historical Failure Rate


Suggested order:

    Authentication
        |
        v
    Authorization
        |
        v
    Critical Workflows
        |
        v
    Data Operations
        |
        v
    External Integrations
        |
        v
    Secondary Features


# 47. Risk-Based Testing

Do not distribute test effort equally.

High-risk areas deserve:

    More Test Cases
    More Negative Testing
    More Role Testing
    More Boundary Testing
    More Regression Coverage


# 48. Test Coverage

Track:

    Features
    Endpoints
    User Roles
    Workflows
    Browsers / Clients
    Environments
    Test Cases
    Pass Rate
    Failure Rate


Do not claim:

    "Fully Tested"


unless the defined scope and coverage justify it.


# 49. Coverage Gaps

Document:

    Untested Features
    Unavailable APIs
    Missing Credentials
    Broken Environment
    Unsupported Browser
    Unavailable Integrations


Coverage gaps must appear in the final assessment.


# 50. Test Execution Strategy

Use progressive execution:

    Smoke
        |
        v
    Critical Functional Tests
        |
        v
    API / Integration Tests
        |
        v
    Negative Tests
        |
        v
    E2E Tests
        |
        v
    Regression


Adjust based on:

    Project
    Scope
    Risk
    Time


# 51. Tool Selection

Follow:

    .claude/rules/tool-selection.md


General mapping:

    Functional Testing
        ->
    TestSprite

    E2E Testing
        ->
    TestSprite

    API Testing
        ->
    TestSprite
        +
    Relevant Project Tools

    Static Security Analysis
        ->
    Code Review Agent

    Dynamic Security Testing
        ->
    Security Agent


Do not force QA tools into security tasks.


# 52. TestSprite Failure Handling

If TestSprite fails:

    Record the failure.

Document:

    Operation
    Target
    Error
    Impact


Do not fabricate:

    Test Results
    Coverage
    Passed Tests


If another authorized testing mechanism is available:

    Use it when appropriate.


# 53. Test Result Integrity

A test may be:

    PASS
    FAIL
    BLOCKED
    SKIPPED
    INCONCLUSIVE


Do not convert:

    BLOCKED

into:

    PASS


Do not convert:

    INCONCLUSIVE

into:

    FAIL


# 54. Pass Criteria

A test passes only when:

    Expected Behavior

matches:

    Observed Behavior


and:

    Relevant Preconditions

were satisfied.


# 55. Failure Criteria

A test fails when:

    Preconditions are valid

and:

    Observed Behavior

does not match:

    Expected Behavior.


# 56. Inconclusive Criteria

Use:

    INCONCLUSIVE


when:

    Evidence is insufficient.

Examples:

    Environment instability
    Partial execution
    Missing dependency
    Ambiguous behavior


Inconclusive results require follow-up when important.


# 57. Blocked Criteria

Use:

    BLOCKED


when:

    The test cannot execute because of a known external constraint.


Example:

    Required test account unavailable.


# 58. Security Evidence Quality

A QA result becomes stronger security evidence when it includes:

    Known User Role
    Controlled Resource
    Expected Authorization
    Actual Response
    Reproducible Steps
    Concrete Evidence


Avoid vague statements such as:

    "The API seems insecure."


# 59. Reproducibility

A failed test should ideally be reproducible.

Record:

    Preconditions
    Exact Steps
    Input
    Role
    Endpoint
    Expected Result
    Actual Result


A reproducible failure is easier to validate and remediate.


# 60. Regression After Fix

When a defect or vulnerability is fixed:

    Re-run Original Test


Then:

    Run Related Tests


Finally:

    Run Critical Regression Suite


This verifies both:

    Fix Correctness

and:

    Functional Stability


# 61. QA Reporting

QA results should provide:

    Executive Summary
    Test Scope
    Environment
    Test Strategy
    Coverage
    Results
    Failed Tests
    Blocked Tests
    Security-Relevant Observations
    Limitations
    Regression Status


The final professional report is generated by:

    .claude/agents/report-generator.md


# 62. Reporting Separation

The QA layer should distinguish:

    Test Result


from:

    Security Finding


A failed test is not automatically a vulnerability.

The Vulnerability Analyst owns:

    Security Finding Validation


# 63. Report Handoff

The normal flow is:

    QA Agent
        |
        v
    QA Testing Skill
        |
        v
    TestSprite
        |
        v
    Results
        |
        +----------------------+
        |                      |
        v                      v
 Functional Defects    Security-Relevant
                              Results
        |                      |
        |                      v
        |              Vulnerability Analyst
        |                      |
        +----------+-----------+
                   |
                   v
            Report Generator


# 64. Cross-Agent Correlation

QA results should be correlated with:

    Code Review Findings

and:

    Dynamic Security Findings


Example:

    Code Review:
        Missing ownership check.

    QA:
        Unauthorized resource access observed.

    Dynamic Security:
        API accepts foreign object ID.


These may represent:

    One Validated Finding

with:

    Multiple Evidence Sources.


# 65. Avoid Duplicate Findings

Do not create separate security findings for:

    Same Root Cause
    Same Attack Path
    Same Component
    Same Impact


Allow the Vulnerability Analyst to consolidate evidence.


# 66. Business-Critical Testing

Identify critical workflows such as:

    Authentication
    Payments
    Account Management
    Permissions
    Data Export
    File Upload
    Administration
    User Management


These should receive higher testing priority.


# 67. Availability Considerations

Avoid tests that may:

    Exhaust Resources
    Trigger Large Costs
    Lock Accounts
    Delete Data
    Disrupt Services


unless explicitly authorized.


# 68. Production Safety

For production environments:

    Prefer Read-Only Testing

where possible.

Avoid:

    Destructive Actions
    High-Volume Requests
    Account Lockout
    Real Payment Operations
    Data Deletion


Use controlled test accounts and data.


# 69. Sensitive Data Handling

Do not unnecessarily collect:

    Passwords
    Tokens
    API Keys
    Personal Data
    Payment Information


Redact sensitive information from:

    Screenshots
    Logs
    Reports
    Test Artifacts


# 70. Test Environment Reset

When tests modify state:

    Restore Test State


when practical.

Example:

    Create Test User
        |
        v
    Execute Test
        |
        v
    Validate
        |
        v
    Cleanup


Do not delete unrelated data.


# 71. Cleanup Failures

If cleanup fails:

    Record the failure.


Do not silently ignore:

    Residual Test Data


Especially when it may affect:

    Security
    Privacy
    Cost
    Subsequent Tests


# 72. Test Isolation

Tests should avoid depending on:

    Unrelated Previous Tests


Prefer:

    Independent Preconditions


When dependencies are necessary:

    Document Them.


# 73. Flaky Tests

A flaky test is one that:

    Passes and fails unpredictably


Do not classify a flaky test as:

    Confirmed Application Failure


until reproducibility is established.


Track:

    Retry Count
    Environment
    Timing
    Dependencies


# 74. Retry Policy

Retries may be used for:

    Transient Environment Failures


Do not repeatedly retry:

    Deterministic Application Failures


Retries must not hide real defects.


# 75. Test Quality Gate

Before considering QA execution complete:

    [ ] Scope defined
    [ ] Environment confirmed
    [ ] Critical workflows identified
    [ ] Test strategy created
    [ ] Test plan generated
    [ ] Critical tests executed
    [ ] Failures analyzed
    [ ] Flaky tests identified
    [ ] Blocked tests documented
    [ ] Coverage gaps documented
    [ ] Security-relevant failures identified
    [ ] Security evidence handed off
    [ ] Regression performed where required


# 76. Security Correlation Gate

Before finalizing QA results:

    [ ] Authentication failures reviewed
    [ ] Authorization failures reviewed
    [ ] Sensitive-data exposure reviewed
    [ ] Privilege-related failures reviewed
    [ ] Workflow bypasses reviewed
    [ ] Tenant-isolation failures reviewed
    [ ] Unexpected access reviewed
    [ ] Security-relevant API failures reviewed


Potential security issues must reach:

    Vulnerability Analyst


# 77. Completion Criteria

QA testing is complete when:

    The defined test scope was executed
    to the planned depth,

and:

    Results are classified,

and:

    Important failures are analyzed,

and:

    Security-relevant observations are handed off,

and:

    Coverage limitations are documented.


Completion does not mean:

    "The application has no bugs."


# 78. Final QA Principle

The QA Testing Skill exists to answer:

    "Does the application behave correctly
     under expected, unexpected, and boundary conditions?"


And when behavior violates a security boundary:

    "Does this behavior represent
     a potential security vulnerability?"


The final security answer remains with:

    Vulnerability Analyst


The framework responsibilities remain:

    Security Orchestrator
        ->
    Controls the engagement

    QA Agent
        ->
    Controls QA execution

    TestSprite
        ->
    Executes / supports QA testing

    Security Agent
        ->
    Performs dynamic security testing

    Code Review Agent
        ->
    Performs source-code security analysis

    Vulnerability Analyst
        ->
    Correlates and validates security findings

    Report Generator
        ->
    Produces professional reports


The ultimate objective is:

    Reliable Software
        +
    Reliable Security Evidence
        +
    Reproducible Testing
        +
    Clear Separation Between
    Functional Defects and Security Vulnerabilities


The framework should never optimize for:

    Number of Tests


It should optimize for:

    Meaningful Coverage
        +
    Reliable Results
        +
    Security-Relevant Evidence
        +
    Actionable Engineering Feedback.