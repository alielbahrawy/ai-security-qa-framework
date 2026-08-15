# Testing Strategy

## Purpose

This document defines the testing strategy used by the AI Security & QA Engineering Framework.

Its purpose is to establish a unified approach for:

    Security Testing
    Functional Testing
    API Testing
    Integration Testing
    End-to-End Testing
    Regression Testing
    Negative Testing
    Validation Testing
    Security Regression Testing


The framework does not treat testing as a collection
of independent tool executions.

Testing is an evidence-generation process.

The target flow is:

    Application
        |
        v
    Test Strategy
        |
        v
    Test Hypotheses
        |
        v
    Test Execution
        |
        v
    Evidence
        |
        v
    Analysis
        |
        v
    Validation
        |
        v
    Actionable Result


This document works together with:

    .claude/agents/security-orchestrator.md
    .claude/agents/security-agent.md
    .claude/agents/code-review-agent.md
    .claude/agents/qa-agent.md
    .claude/agents/vulnerability-analyst.md
    .claude/agents/report-generator.md

And the framework rules:

    .claude/rules/tool-selection.md
    .claude/rules/workflow.md
    .claude/rules/severity-model.md

And the operational skills:

    .claude/skills/security-audit/SKILL.md
    .claude/skills/code-review/SKILL.md
    .claude/skills/qa-testing/SKILL.md
    .claude/skills/reporting/SKILL.md


# 1. Core Testing Philosophy

The framework follows five principles:

    1. Understand before testing.
    2. Test the highest-risk paths first.
    3. Prefer evidence over assumptions.
    4. Correlate results across testing layers.
    5. Never confuse a test failure with a vulnerability.


Testing should answer questions.

Examples:

    Can an unauthenticated user access this resource?

    Can User A access User B's object?

    Can untrusted input reach a dangerous sink?

    Does the API enforce the documented business rule?

    Does the frontend correctly handle an invalid response?

    Does a security control remain effective
    under abnormal input?


# 2. Testing Layers

The framework uses multiple complementary layers.

    ┌───────────────────────────────────────┐
    │           End-to-End Testing          │
    ├───────────────────────────────────────┤
    │          Integration Testing          │
    ├───────────────────────────────────────┤
    │             API Testing               │
    ├───────────────────────────────────────┤
    │          Component Testing            │
    ├───────────────────────────────────────┤
    │             Unit Testing              │
    ├───────────────────────────────────────┤
    │           Static Analysis             │
    └───────────────────────────────────────┘

Security testing cuts across
all of these layers.


# 3. Testing Categories

## 3.1 Functional Testing

Verifies that expected application
behavior works correctly.

Examples:

    Login
    Registration
    Search
    CRUD
    Checkout
    File Upload
    Account Management


## 3.2 Negative Testing

Verifies behavior when inputs,
states, or permissions are invalid.

Examples:

    Invalid Credentials
    Missing Fields
    Invalid Types
    Expired Tokens
    Unauthorized Requests
    Malformed Payloads


## 3.3 Security Testing

Evaluates whether security boundaries
can be violated.

Examples:

    Authentication
    Authorization
    Input Validation
    Session Management
    Access Control
    Data Protection


## 3.4 Regression Testing

Ensures previously fixed behavior
does not regress.

Every confirmed security vulnerability
should produce a candidate regression test
when technically appropriate.


## 3.5 Integration Testing

Verifies interactions between:

    Frontend
    Backend
    Database
    External APIs
    Authentication Services
    Storage
    Queues
    Caches


## 3.6 End-to-End Testing

Validates complete user or attacker
interaction flows across the application.


# 4. Risk-Based Testing

The framework does not test every
feature with equal priority.

Priority should consider:

    Business Criticality
    Security Impact
    Exposure
    Complexity
    Authentication Requirements
    Data Sensitivity
    Historical Failures
    Change Frequency


High-priority areas typically include:

    Authentication
    Authorization
    Payments
    Account Recovery
    Administrative Functions
    Sensitive Data
    File Uploads
    External Integrations
    API Boundaries


# 5. Attack Surface Mapping

Before extensive testing, identify:

    Entry Points
    APIs
    Pages
    Forms
    Authentication Flows
    File Uploads
    External Integrations
    Administrative Interfaces
    Background Jobs
    WebSockets
    Storage


The resulting map becomes
the testing surface.


# 6. Trust Boundary Testing

Every trust boundary should be
treated as a testing candidate.

Examples:

    Browser -> API
    User -> Backend
    API -> Database
    Application -> External Service
    CI -> Deployment
    Container -> Host


For each boundary ask:

    What data crosses it?

    Who controls the data?

    What validation occurs?

    What authorization occurs?

    What assumptions are made?


# 7. Input Testing

All attacker-controlled inputs
should be considered untrusted.

Common input locations:

    URL Parameters
    Query Parameters
    Form Fields
    JSON
    Headers
    Cookies
    Uploaded Files
    WebSocket Messages
    GraphQL Queries
    API Bodies


Input testing should consider:

    Missing
    Empty
    Null
    Unexpected Type
    Boundary Values
    Oversized Values
    Invalid Encoding
    Duplicate Parameters
    Unexpected Structure


Security payloads should be selected
according to the relevant vulnerability
hypothesis.


# 8. Authentication Testing

Authentication testing should cover:

    Login
    Logout
    Registration
    Password Change
    Password Reset
    Account Recovery
    MFA
    Session Creation
    Session Rotation
    Session Expiration


Questions:

    Can authentication be bypassed?

    Can sessions be reused improperly?

    Are authentication errors handled safely?

    Are sensitive actions re-authenticated
    when required?

    Can expired credentials continue working?


# 9. Authorization Testing

Authorization testing is one of
the highest-priority security areas.

Test:

    Anonymous User
    Normal User
    Privileged User
    Administrator


For each protected operation verify:

    Who should access it?

    Who actually accesses it?

    What resource is being accessed?

    Is ownership verified?

    Is role verification performed server-side?


# 10. Object-Level Authorization

For APIs and resource-oriented applications:

    User A
        |
        v
    Resource A

    User A
        |
        X
    Resource B


The framework should explicitly test
cross-user object access where appropriate.


# 11. Function-Level Authorization

Test access to functions belonging
to different privilege levels.

Example:

    Normal User
        |
        X
    Admin Endpoint


Authorization must be enforced
at the server boundary.


# 12. API Testing Strategy

API testing should examine:

    Endpoint Discovery
    Authentication
    Authorization
    Input Validation
    Output Validation
    Error Handling
    Rate Limiting
    Pagination
    Filtering
    Sorting
    State Transitions


For every relevant endpoint identify:

    Method
    Path
    Authentication
    Required Role
    Input
    Output
    Side Effects


# 13. API Contract Testing

Compare:

    Documentation
        vs
    Implementation


Look for:

    Missing Validation
    Undocumented Fields
    Excessive Response Data
    Incorrect Status Codes
    Unexpected Methods
    Authorization Inconsistencies


# 14. Frontend Testing

Frontend testing should cover:

    Rendering
    Navigation
    Forms
    Authentication State
    Error Handling
    Loading States
    Authorization UX
    Input Validation


Important rule:

Frontend controls are not security boundaries.

A hidden button does not equal
server-side authorization.


# 15. Backend Testing

Backend testing should focus on:

    Business Logic
    Authorization
    Validation
    State Management
    Database Interaction
    External Service Interaction
    Error Handling


The backend is generally the
primary enforcement boundary.


# 16. Database Testing

Review interactions involving:

    Queries
    ORM
    Transactions
    Constraints
    Access Control
    Sensitive Data
    Migrations


Security-oriented checks include:

    Injection
    Excessive Privileges
    Data Exposure
    Integrity Violations


# 17. File Upload Testing

For upload functionality test:

    File Type
    Extension
    MIME Type
    File Size
    File Name
    File Content
    Storage Location
    Retrieval
    Execution Behavior


Do not assume extension validation
is equivalent to content validation.


# 18. Session Testing

Review:

    Session Creation
    Rotation
    Expiration
    Revocation
    Cookie Attributes
    Concurrent Sessions


Security-sensitive state transitions
should be explicitly tested.


# 19. Token Testing

For tokens test:

    Creation
    Validation
    Expiration
    Rotation
    Revocation
    Scope
    Audience
    Issuer


Do not trust client-provided
token claims without server validation.


# 20. Error Handling Testing

Test:

    Invalid Input
    Missing Resources
    Unauthorized Access
    Server Errors
    Dependency Failures


Check whether errors expose:

    Stack Traces
    Secrets
    Internal Paths
    Database Details
    Infrastructure Information


# 21. Business Logic Testing

Business logic testing begins
with understanding intended rules.

For each workflow identify:

    Preconditions
    Action
    State Change
    Postconditions


Then test:

    Missing Preconditions
    Repeated Actions
    Reordered Actions
    Boundary Values
    Concurrent Actions
    Unauthorized Actions


# 22. State Transition Testing

Applications should be tested
as state machines where appropriate.

Example:

    CREATED
       |
       v
    VERIFIED
       |
       v
    ACTIVE
       |
       v
    SUSPENDED


Test whether invalid transitions
are rejected.

Example:

    SUSPENDED
        |
        X
    Sensitive Operation


# 23. Boundary Testing

Important boundaries include:

    Minimum
    Maximum
    Zero
    Negative
    Empty
    Null
    Maximum Length
    Large Payload
    Expired Time
    Future Time


Boundary tests are especially
valuable for business logic.


# 24. Concurrency Testing

Concurrency testing is relevant
when operations modify shared state.

Examples:

    Payment
    Inventory
    Coupon Redemption
    Token Usage
    Permission Changes


The objective is to detect
security-relevant race conditions.


# 25. Dependency Testing

External dependencies should be
tested for failure behavior.

Examples:

    Payment Provider
    Email Provider
    Authentication Provider
    Storage
    Database
    Third-Party API


Questions:

    What happens if it times out?

    What happens if it returns invalid data?

    What happens if it becomes unavailable?

    Does failure bypass a security control?


# 26. Security Regression Testing

Every validated vulnerability
should be evaluated for regression coverage.

Flow:

    Vulnerability
        |
        v
    Root Cause
        |
        v
    Regression Test
        |
        v
    Future Test Suite


This prevents the framework
from rediscovering the same
problem repeatedly.


# 27. Static-to-Dynamic Testing

Static findings should generate
dynamic testing hypotheses where useful.

Example:

    Code Review
        |
        v
    User Input -> SQL Query
        |
        v
    Dynamic Validation
        |
        v
    Evidence


Static analysis identifies
potential weaknesses.

Dynamic testing determines
whether behavior is actually
security-relevant.


# 28. Dynamic-to-Static Testing

Dynamic findings should trigger
source investigation where available.

Example:

    Dynamic Test
        |
        v
    Unauthorized Access
        |
        v
    Code Review
        |
        v
    Authorization Root Cause


This improves remediation quality.


# 29. QA-to-Security Correlation

A functional test failure may
reveal a security problem.

Example:

    QA Test
        |
        v
    User A sees User B data
        |
        v
    Security Finding


The QA Agent should therefore
surface suspicious security-relevant
behavior to the Vulnerability Analyst.


# 30. Security-to-QA Correlation

A security finding may require
functional regression coverage.

Example:

    IDOR Finding
        |
        v
    Authorization Regression Test
        |
        v
    QA Test Suite


Security and QA are therefore
connected feedback loops.


# 31. TestSprite Role

TestSprite is the QA/functional
testing layer of the framework.

It should primarily support:

    Project Bootstrap
    Codebase Understanding
    Test Planning
    Frontend Testing
    Backend Testing
    Functional Testing
    End-to-End Testing
    Regression Testing
    Result Analysis


TestSprite is not treated as
the primary security scanner.


# 32. Security Tool Role

Security-oriented tools such as:

    pentest-ai
    PentesterFlow
    Semgrep


should be selected according to:

    Target
    Test Type
    Evidence Required
    Risk
    Scope


Tool selection belongs to:

    .claude/rules/tool-selection.md


# 33. Test Planning

Before executing a broad test campaign,
the framework should produce:

    Scope
    Target
    Risk Areas
    Test Categories
    Priority
    Expected Evidence
    Constraints


The plan should be proportional
to the application.


# 34. Test Case Structure

A meaningful test case should contain:

    ID
    Objective
    Preconditions
    Input
    Action
    Expected Result
    Security Expectation
    Evidence
    Result


Example:

    ID:
        AUTHZ-001

    Objective:
        Verify object-level authorization.

    Preconditions:
        Two authenticated users exist.

    Action:
        User A requests User B's resource.

    Expected Result:
        Access denied.

    Security Expectation:
        Server enforces ownership.

    Evidence:
        HTTP response and application behavior.


# 35. Test Result Classification

Use:

    PASS
    FAIL
    BLOCKED
    NOT_APPLICABLE
    INCONCLUSIVE


Do not convert:

    FAIL

directly into:

    VULNERABILITY


The Vulnerability Analyst must
evaluate the evidence.


# 36. Evidence Quality

Strong evidence may include:

    Reproducible Request
    Reproducible Response
    Source Location
    Stack Trace
    Test Output
    Screenshot
    Log Evidence
    Data Flow
    State Transition


Weak evidence includes:

    Guess
    Generic Scanner Warning
    Unverified Assumption
    Theoretical Possibility


# 37. Reproducibility

A confirmed security issue
should ideally be reproducible.

Record:

    Preconditions
    Request
    Input
    Sequence
    Expected Result
    Actual Result


Avoid relying on undocumented
manual steps whenever possible.


# 38. Test Isolation

Tests should avoid contaminating
other tests.

Prefer:

    Dedicated Test Data
    Isolated Accounts
    Resettable State
    Deterministic Inputs


Especially important for:

    Payments
    Authentication
    Authorization
    Data Modification


# 39. Test Data Strategy

Test data should represent:

    Anonymous Users
    Normal Users
    Privileged Users
    Administrators
    Invalid Users
    Expired Sessions
    Different Ownership States


Sensitive production data should
not be unnecessarily exposed
during testing.


# 40. Safe Active Testing

Active testing must respect:

    Authorization
    Scope
    Rate Limits
    Production Stability
    Data Integrity


Avoid destructive tests unless
explicitly authorized and controlled.


# 41. Test Ordering

The preferred order is:

    1. Understand Application
    2. Map Attack Surface
    3. Identify Critical Flows
    4. Run Low-Risk Tests
    5. Analyze Results
    6. Run Targeted Security Tests
    7. Validate Findings
    8. Run Regression Tests
    9. Report


Do not begin with maximum-intensity
testing without understanding the target.


# 42. Test Prioritization

Priority levels:

    CRITICAL
    HIGH
    MEDIUM
    LOW
    INFORMATIONAL


These labels indicate testing priority,
not vulnerability severity.

Final vulnerability severity belongs to:

    .claude/rules/severity-model.md


# 43. Test Coverage

Coverage should be measured across:

    Features
    Endpoints
    Roles
    States
    Input Classes
    Security Controls
    Critical Workflows


High code coverage does not guarantee
high security coverage.


# 44. Coverage Gaps

The framework should explicitly identify:

    Untested Endpoints
    Untested Roles
    Untested Workflows
    Blocked Tests
    Unknown Components
    Missing Environment Information


Unknown should remain unknown.

Do not infer PASS from absence of failure.


# 45. Test Failure Analysis

When a test fails:

    Reproduce
        |
        v
    Determine Cause
        |
        v
    Classify Failure
        |
        v
    Determine Security Relevance


Possible causes:

    Application Bug
    Test Bug
    Environment Issue
    Dependency Failure
    Expected Behavior
    Security Control Failure


# 46. Flaky Tests

Flaky behavior should not
automatically become a vulnerability.

Investigate:

    Timing
    Race Conditions
    External Services
    Environment
    Test Isolation
    State


If security impact exists,
escalate to validation.


# 47. Test Evidence Handoff

The QA Agent should return
structured information to the
Vulnerability Analyst.

Minimum information:

    Test ID
    Target
    Expected Behavior
    Actual Behavior
    Reproduction Steps
    Evidence
    Security Relevance
    Confidence


# 48. Security Evidence Handoff

The Security Agent should return:

    Finding Candidate
    Affected Component
    Attack Surface
    Technique
    Evidence
    Reproduction
    Impact Hypothesis
    Confidence


The Vulnerability Analyst
correlates these results.


# 49. Code Review Handoff

The Code Review Agent should return:

    File
    Location
    Data Flow
    Vulnerable Pattern
    Security Control
    Candidate Impact
    Suggested Validation


This allows dynamic testing
to validate static hypotheses.


# 50. Orchestrator Integration

The Security Orchestrator owns
the high-level testing strategy.

It should:

    Understand Scope
        |
        v
    Identify Relevant Test Layers
        |
        v
    Select Appropriate Agents
        |
        v
    Select Appropriate Tools
        |
        v
    Coordinate Execution
        |
        v
    Collect Evidence
        |
        v
    Request Correlation
        |
        v
    Trigger Reporting


The Orchestrator should not
duplicate specialized agent work.


# 51. Vulnerability Analyst Integration

The Vulnerability Analyst receives:

    Static Findings
    Security Test Results
    QA Results
    Runtime Evidence
    Code Evidence


It then determines:

    Duplicate?
    Real?
    Reproducible?
    Security-Relevant?
    What is the Root Cause?
    What is the Impact?
    What Evidence Supports It?


Only validated findings proceed
to final reporting.


# 52. Report Generator Integration

The Report Generator consumes
validated findings.

It should not independently
decide whether a scanner output
is a vulnerability.

Its role is:

    Organize
    Explain
    Contextualize
    Document
    Recommend


# 53. Continuous Feedback Loop

The framework should operate
as a feedback system:

    Discovery
        |
        v
    Testing
        |
        v
    Findings
        |
        v
    Validation
        |
        v
    Remediation
        |
        v
    Regression Testing
        |
        v
    Updated Knowledge


This allows future assessments
to become more efficient.


# 54. Test Reuse

Reusable tests should be retained
when they are:

    Deterministic
    Relevant
    Maintainable
    Valuable


Reusable security regression tests
are especially important.


# 55. Test Naming

Tests should use predictable names.

Examples:

    AUTH-001
    AUTHZ-001
    API-001
    INPUT-001
    SESSION-001
    BUSINESS-001
    FILE-001
    SSRF-001


Security regression tests may use:

    SEC-REG-001


# 56. Test Traceability

Each important test should be traceable
to one or more:

    Requirement
    Feature
    Security Control
    Vulnerability
    Risk
    Agent Finding


Example:

    SEC-REG-004
        |
        v
    IDOR-001
        |
        v
    Authorization Control


# 57. Finding-to-Test Traceability

Every validated vulnerability
should answer:

    Was it tested?

    What evidence confirmed it?

    Can it be reproduced?

    Does a regression test exist?

    If not, why?


# 58. Security Test Escalation

Testing should escalate gradually.

Example:

    Passive Analysis
        |
        v
    Static Analysis
        |
        v
    Safe Functional Testing
        |
        v
    Targeted Security Testing
        |
        v
    Controlled Validation


The framework should not jump
directly to aggressive testing
without justification.


# 59. Stopping Conditions

Testing may stop when:

    Scope Is Exhausted
    Critical Paths Are Covered
    Evidence Is Sufficient
    Risk Is Controlled
    Further Testing Has Low Expected Value


The framework should optimize
for useful evidence, not test count.


# 60. Unknowns

If the framework cannot determine:

    Authentication Model
    Deployment Architecture
    API Behavior
    Permission Model
    Data Sensitivity


it should record the uncertainty
and request clarification when needed.

Unknown is not equivalent to safe.


# 61. Final Testing Pipeline

The complete testing pipeline is:

    ┌───────────────────────────────┐
    │        Application            │
    └───────────────┬───────────────┘
                    |
                    v
    ┌───────────────────────────────┐
    │     Application Discovery     │
    └───────────────┬───────────────┘
                    |
                    v
    ┌───────────────────────────────┐
    │       Attack Surface Map      │
    └───────────────┬───────────────┘
                    |
                    v
    ┌───────────────────────────────┐
    │        Test Strategy          │
    └───────────────┬───────────────┘
                    |
          ┌─────────┼─────────┐
          |         |         |
          v         v         v
       Static     Security     QA
       Analysis   Testing     Testing
          |         |         |
          └─────────┼─────────┘
                    |
                    v
    ┌───────────────────────────────┐
    │       Evidence Correlation    │
    │     Vulnerability Analyst     │
    └───────────────┬───────────────┘
                    |
                    v
    ┌───────────────────────────────┐
    │       Validated Findings      │
    └───────────────┬───────────────┘
                    |
                    v
    ┌───────────────────────────────┐
    │       Severity Model          │
    └───────────────┬───────────────┘
                    |
                    v
    ┌───────────────────────────────┐
    │        Report Generator       │
    └───────────────┬───────────────┘
                    |
                    v
    ┌───────────────────────────────┐
    │      Security / QA Report     │
    └───────────────┬───────────────┘
                    |
                    v
    ┌───────────────────────────────┐
    │       Regression Tests        │
    └───────────────────────────────┘


# 62. Final Principle

The objective of testing is not:

    "Run every available tool."

The objective is:

    "Generate enough reliable evidence
     to understand whether the system
     behaves securely and correctly."


Therefore:

    Test Intelligently
        |
        v
    Collect Evidence
        |
        v
    Correlate Results
        |
        v
    Validate Findings
        |
        v
    Report Clearly
        |
        v
    Prevent Regression


This is the testing foundation
of the entire framework.