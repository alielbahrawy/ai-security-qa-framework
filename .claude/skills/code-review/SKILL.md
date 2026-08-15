---
name: code-review
description: Perform security-focused source-code review using static analysis, semantic reasoning, framework-aware inspection, and Semgrep when appropriate. Produce evidence-backed candidate findings for the Vulnerability Analyst rather than treating static-analysis results as confirmed vulnerabilities.
---

# Code Review Skill

## 1. Purpose

This skill defines the operational process for security-focused code review within the AI Security & QA Engineering Framework.

Its purpose is to transform source code into:

    Codebase
        |
        v
    Security Understanding
        |
        v
    Static Analysis
        |
        v
    Manual / Semantic Review
        |
        v
    Candidate Findings
        |
        v
    Vulnerability Analyst
        |
        v
    Validated Findings
        |
        v
    Report Generator


The skill focuses on:

    Source-Code Security
    Application Architecture
    Security Controls
    Data Flow
    Trust Boundaries
    Vulnerability Patterns
    Business Logic
    Dependency Risk
    Configuration Security


This skill does not independently declare final vulnerabilities.

The final security truth remains with:

    .claude/agents/vulnerability-analyst.md


# 2. Framework Position

The Code Review Skill operates as part of:

    Claude Code
         |
         v
    Security Orchestrator
         |
         v
    Code Review Agent
         |
         v
    Code Review Skill
         |
    +----+--------------------+
    |                         |
    v                         v
Semgrep                Manual Analysis
    |                         |
    +------------+------------+
                 |
                 v
        Candidate Findings
                 |
                 v
      Vulnerability Analyst
                 |
                 v
        Validated Findings
                 |
                 v
        Report Generator


The Security Orchestrator decides:

    Whether code review is required.

The Code Review Agent decides:

    How the review should be performed.

This skill defines:

    How the code should be analyzed.


# 3. Required Framework Context

Before performing a security code review, use:

    .claude/CLAUDE.md

    .claude/agents/security-orchestrator.md
    .claude/agents/code-review-agent.md
    .claude/agents/security-agent.md
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


# 4. Review Objectives

The review should determine:

    Where security-sensitive behavior exists.

    Where untrusted data enters the system.

    Where trust boundaries exist.

    Where authorization is enforced.

    Where authentication is implemented.

    Where sensitive data is processed.

    Where dangerous operations occur.

    Whether security controls are correctly implemented.

    Whether controls can be bypassed.

    Whether vulnerabilities can be demonstrated.


The objective is not:

    Reading every line equally.


The objective is:

    Finding security-relevant behavior efficiently and accurately.


# 5. Review Modes

The skill supports:

    Full Security Code Review
    Targeted Security Review
    Pull Request Security Review
    API Security Review
    Authentication Review
    Authorization Review
    Dependency Review
    Secrets Review
    Configuration Review
    Business Logic Review


The review mode is determined by:

    User Request
    Engagement Scope
    Security Orchestrator


# 6. Scope Gate

Before reviewing code, determine:

    Repository
    Branch
    Commit
    Pull Request
    Directory
    Service
    Application
    Environment


Record:

    In-Scope Code
    Out-of-Scope Code
    Generated Code
    Third-Party Code
    Test Code
    Configuration


Do not silently expand scope.


# 7. Codebase Reconnaissance

Before running deep analysis, understand the project.

Identify:

    Programming Languages
    Frameworks
    Runtime
    Package Managers
    Application Entry Points
    API Routes
    Authentication
    Authorization
    Database Layer
    External Services
    File Handling
    Background Jobs
    Message Queues
    Configuration
    Infrastructure Code


Example:

    Frontend
        |
        v
    API
        |
        v
    Service Layer
        |
        v
    Database


The objective is to understand where security decisions occur.


# 8. Repository Structure

Inspect relevant structures such as:

    src/
    app/
    api/
    routes/
    controllers/
    services/
    middleware/
    auth/
    models/
    database/
    config/
    utils/
    workers/
    jobs/
    tests/


Do not assume directory names.

Use actual repository structure.


# 9. Entry-Point Identification

Prioritize:

    HTTP Routes
    API Endpoints
    CLI Commands
    Background Jobs
    Message Consumers
    File Upload Handlers
    Webhook Handlers
    Authentication Handlers
    Administrative Functions


Entry points define where attacker-controlled input may enter.


# 10. Trust Boundary Mapping

Identify transitions such as:

    User
        |
        v
    Browser
        |
        v
    API
        |
        v
    Application
        |
        v
    Database


Also:

    Application
        |
        v
    Operating System


    Application
        |
        v
    External Service


    User Input
        |
        v
    Template Engine


These boundaries are high-value review targets.


# 11. Data Flow Analysis

Trace sensitive data:

    Source
        |
        v
    Validation
        |
        v
    Transformation
        |
        v
    Sink


Potential sources:

    Request Parameters
    JSON Body
    Headers
    Cookies
    Files
    Environment Variables
    Database Records
    External APIs


Potential sinks:

    SQL Query
    Shell Command
    HTML Response
    Template
    File Path
    Redirect
    HTTP Request
    Deserialization
    Logging
    Dynamic Code Execution


# 12. Source-to-Sink Reasoning

For security-sensitive code, determine:

    Where does the data originate?

    Is it trusted?

    Is it validated?

    Is it normalized?

    Is it encoded?

    Is it authorized?

    Is it transformed safely?

    Where does it end up?


Do not assume a variable is safe simply because it has a harmless name.


# 13. Security-Sensitive Operations

Prioritize code involving:

    Authentication
    Authorization
    Session Management
    Cryptography
    Password Handling
    Token Handling
    Database Queries
    Command Execution
    File Operations
    Network Requests
    Serialization
    Deserialization
    HTML Rendering
    Template Rendering
    Redirects
    Access Control
    Privileged Operations


# 14. Semgrep

Use:

    Semgrep


when appropriate for:

    Pattern Detection
    Known Vulnerability Patterns
    Dangerous API Usage
    Secrets
    Injection
    Security Misconfiguration
    Framework-Specific Issues


Semgrep output is:

    Candidate Evidence


It is not automatically:

    Confirmed Vulnerability


# 15. Static Analysis Workflow

Use:

    Repository
        |
        v
    Semgrep
        |
        v
    Candidate Results
        |
        v
    Semantic Review
        |
        v
    Candidate Findings
        |
        v
    Vulnerability Analyst
        |
        v
    Validation


Never skip semantic review for important findings.


# 15A. Semgrep Usage

Semgrep is used when source code is available and static security analysis is relevant.

Code Review Agent owns Semgrep execution.

Execution details are defined in:
    .claude/agents/code-review-agent.md

Selection guidance is defined in:
    .claude/rules/tool-selection.md

Do not assume `semgrep` exists in PATH.

Preserve Semgrep execution status, failures, evidence, limitations, and coverage impact.

Semgrep output is candidate evidence, not a final vulnerability conclusion.

Route security-relevant findings to the Vulnerability Analyst.

Do not assume an MCP or headless mode that has not been verified.


# 16. Tool Selection

Tool selection must follow:

    .claude/rules/tool-selection.md


Do not execute tools merely because they are available.

Use a tool when it provides:

    Relevant Coverage
    Useful Evidence
    Efficient Analysis


Avoid:

    Redundant Tool Execution


# 17. Manual Security Review

Static analysis cannot fully understand:

    Business Logic
    Authorization Context
    Multi-Step Workflows
    Tenant Isolation
    Trust Relationships
    Security Assumptions


Therefore manually inspect:

    Authentication
    Authorization
    Privileged Actions
    Sensitive Data Flows
    Business Logic
    Security Boundaries


# 18. Authentication Review

Inspect:

    Login
    Registration
    Password Reset
    MFA
    Session Creation
    Session Validation
    Logout
    Token Refresh
    OAuth
    API Keys
    Service Authentication


Questions:

    Can authentication be bypassed?

    Are credentials handled securely?

    Are sessions bound correctly?

    Are tokens validated correctly?

    Can expired credentials be reused?


# 19. Authorization Review

Inspect:

    Middleware
    Guards
    Policies
    Permission Checks
    Ownership Checks
    Role Checks
    Tenant Checks


Ask:

    Who is allowed?

    Where is authorization checked?

    Is the check server-side?

    Is it applied consistently?

    Can the check be bypassed?

    Is object ownership verified?


# 20. Horizontal Access Control

Look for patterns where:

    User A
        |
        v
    Object A


can potentially access:

    User B
        |
        v
    Object B


Common indicators:

    Direct Object IDs
    Missing Ownership Checks
    User-Controlled Resource IDs
    Missing Tenant Filters


Potential classification:

    Broken Object-Level Authorization


Validation should be performed before reporting.


# 21. Vertical Access Control

Inspect whether:

    Normal User
        |
        X
    Administrative Function


can be bypassed through:

    Missing Role Checks
    Client-Side Enforcement
    Hidden Endpoints
    Direct API Calls
    Alternate Routes


# 22. Tenant Isolation

For multi-tenant systems, verify:

    Tenant ID
        |
        v
    Authorization
        |
        v
    Database Query


Watch for:

    Missing Tenant Filters
    User-Controlled Tenant IDs
    Cross-Tenant Queries
    Shared Resource Confusion


Tenant isolation issues may have significant impact.


# 23. Input Validation Review

Identify:

    User-Controlled Input


Then determine:

    Validation
    Sanitization
    Normalization
    Encoding


before:

    Sensitive Sink


Do not assume:

    "Frontend validates it"


is sufficient security control.


# 24. SQL Injection

Inspect:

    Raw Queries
    Query Construction
    String Concatenation
    Dynamic SQL
    ORM Escape-Hatch APIs


Prefer:

    Parameterized Queries


Potential evidence:

    User Input
        |
        v
    String Construction
        |
        v
    SQL Execution


Static evidence alone may require runtime validation.


# 25. Command Injection

Inspect:

    Shell Execution
    Process Spawning
    System Commands
    Dynamic Arguments


Trace:

    User Input
        |
        v
    Command Construction
        |
        v
    Process Execution


Assess:

    Input Control
    Argument Separation
    Allowlisting
    Execution Context


Do not execute destructive commands merely to validate a finding.


# 26. Path Traversal

Inspect:

    File Paths
    Downloads
    Uploads
    Archive Extraction
    File Reads
    File Writes


Potential flow:

    User Input
        |
        v
    File Path
        |
        v
    File Operation


Check:

    Canonicalization
    Path Validation
    Allowlisting
    Root Directory Enforcement


# 27. SSRF

Inspect server-side network requests using:

    User-Controlled URLs
    Redirects
    Webhooks
    Proxy Functions
    Import Functions


Potential flow:

    User Input
        |
        v
    Server HTTP Client
        |
        v
    Internal Resource


Review:

    URL Validation
    Host Allowlisting
    Redirect Handling
    Network Segmentation


# 28. XSS

Inspect:

    HTML Rendering
    Template Rendering
    DOM Manipulation
    Raw HTML APIs


Potential flow:

    Untrusted Input
        |
        v
    HTML Context
        |
        v
    Browser


Determine:

    Context
    Encoding
    Sanitization
    Framework Protections


Do not assume framework protection is universal.


# 29. Template Injection

Inspect:

    User-Controlled Template Content


feeding:

    Template Engine


Review:

    Template Compilation
    Expression Evaluation
    Dynamic Rendering


Static detection may require runtime validation.


# 30. Deserialization

Inspect:

    Object Deserialization
    Pickle-like Mechanisms
    Dynamic Object Reconstruction
    Unsafe Serialization Formats


Determine:

    Input Trust
    Gadget Exposure
    Execution Capability


Treat unsafe deserialization as high-risk when evidence supports it.


# 31. File Upload

Review:

    Filename Validation
    MIME Validation
    Extension Validation
    Storage Location
    Execution Permissions
    Content Validation
    Access Control


Check whether uploaded files can become:

    Executable
    Publicly Accessible
    Path Traversal Payloads


# 32. Secrets

Search for:

    API Keys
    Passwords
    Tokens
    Private Keys
    Connection Strings
    Cloud Credentials


Potential locations:

    Source Code
    Configuration
    CI/CD
    Docker Files
    Scripts
    Test Files


Classify findings carefully.

A test credential is not equivalent to:

    Production Credential


# 33. Cryptography

Review:

    Password Hashing
    Encryption
    Signing
    Key Management
    Randomness
    TLS Configuration


Look for:

    Weak Algorithms
    Hardcoded Keys
    Predictable Randomness
    Improper Key Storage
    Missing Authentication
    Incorrect Verification


Do not report an algorithm as vulnerable solely because it is old without considering its actual use.


# 34. Password Storage

Preferred pattern:

    Password
        |
        v
    Strong Password Hash
        |
        v
    Secure Storage


Inspect:

    Hash Algorithm
    Salt
    Work Factor
    Password Reset
    Credential Exposure


Plaintext password storage is a critical security concern.


# 35. Session Management

Inspect:

    Session Creation
    Session Rotation
    Expiration
    Revocation
    Cookie Flags
    Token Storage


Relevant controls include:

    Secure
    HttpOnly
    SameSite


Evaluate according to application architecture.


# 36. JWT / Token Review

Inspect:

    Signature Verification
    Algorithm Handling
    Expiration
    Issuer
    Audience
    Key Management
    Token Storage


Look for:

    Missing Verification
    Weak Validation
    Trusting Client Claims
    Missing Expiration


# 37. OAuth Review

Inspect:

    Redirect URI Validation
    State Parameter
    PKCE
    Token Handling
    Client Authentication
    Scope Validation


Pay particular attention to:

    Authorization Code Flow


and:

    Redirect Boundaries


# 38. API Key Review

Inspect:

    Creation
    Storage
    Transmission
    Rotation
    Revocation
    Scope


Avoid exposing keys through:

    Source Code
    Client Bundles
    Logs
    Error Messages


# 39. Logging

Review whether logs contain:

    Passwords
    Tokens
    Session IDs
    Personal Data
    Secrets


Also determine whether security events are logged appropriately.


# 40. Error Handling

Inspect:

    Exception Responses
    Stack Traces
    Debug Modes
    Error Messages


Potential information disclosure includes:

    Internal Paths
    Database Errors
    Framework Versions
    Secrets
    Infrastructure Details


# 41. Security Headers

Where applicable, inspect:

    Content-Security-Policy
    Strict-Transport-Security
    X-Content-Type-Options
    Referrer-Policy
    Permissions-Policy
    Frame Restrictions


Do not report missing headers without considering:

    Application Architecture
    Threat Model
    Existing Controls


# 42. CORS

Review:

    Allowed Origins
    Credentials
    Methods
    Headers


Look for dangerous combinations such as:

    Broad Origin Trust
        +
    Credentialed Requests


Validate actual exploitability.


# 43. CSRF

Inspect state-changing requests.

Determine whether protection exists through:

    SameSite Cookies
    CSRF Tokens
    Origin Validation
    Framework Controls


Do not report CSRF when the application's authentication architecture makes the threat inapplicable without evidence.


# 44. Rate Limiting

Inspect sensitive operations:

    Login
    Password Reset
    OTP
    API Keys
    Resource Creation
    Expensive Queries


Determine whether controls exist where abuse could matter.


# 45. Race Conditions

Inspect:

    Balance Updates
    Inventory
    Transactions
    Permissions
    State Changes
    Token Usage


Look for:

    Check
        |
        v
    Time Gap
        |
        v
    Use


Race-condition findings usually require dynamic validation.


# 46. Business Logic

Review workflows rather than only individual functions.

Example:

    Create
        |
        v
    Approve
        |
        v
    Execute


Ask:

    Can a step be skipped?

    Can states be manipulated?

    Can actions be repeated?

    Can users perform actions out of order?

    Can limits be bypassed?


# 47. Dependency Review

Inspect:

    package.json
    requirements.txt
    pyproject.toml
    go.mod
    pom.xml
    Gemfile
    composer.json
    Dockerfiles
    Lock Files


When identifying vulnerable dependencies, verify:

    Package
    Version
    Affected Range
    Actual Usage
    Exposure


Do not treat every outdated dependency as an exploitable vulnerability.


# 48. Configuration Review

Inspect:

    Environment Configuration
    Debug Mode
    Authentication Settings
    Database Configuration
    CORS
    TLS
    Storage
    Cloud Configuration


Distinguish:

    Development Configuration

from:

    Production Configuration


# 49. Infrastructure-as-Code

When in scope, inspect:

    Docker
    Kubernetes
    Terraform
    CloudFormation
    CI/CD


Potential issues:

    Public Resources
    Excessive Permissions
    Hardcoded Secrets
    Insecure Defaults
    Privileged Containers
    Weak Network Isolation


# 50. Frontend Security Review

Inspect:

    Client-Side Authorization
    Sensitive Data Exposure
    Token Storage
    DOM Manipulation
    API Calls
    Source Maps
    Environment Variables


Important principle:

    Client-Side Controls

must not be treated as:

    Server-Side Authorization


# 51. Mobile / Client Applications

When applicable, inspect:

    API Keys
    Token Storage
    Local Storage
    Certificate Validation
    Debug Configuration
    Sensitive Data


Client applications should be treated as:

    Untrusted Environments


# 52. Test Code Review

Test code may reveal:

    Hardcoded Credentials
    Security Bypass Helpers
    Mocked Authorization
    Disabled Validation


However:

    Test-only behavior

must not automatically be reported as:

    Production Vulnerability


Determine whether the code path reaches production.


# 53. Generated Code

Identify:

    Generated Files
    Build Artifacts
    Vendor Code


Avoid reporting generated artifacts when the actual source control is elsewhere.

Trace issues back to:

    Source Configuration
    Template
    Dependency


when possible.


# 54. Candidate Finding Structure

Every candidate finding should contain:

    Finding ID
    Source
    File
    Line
    Component
    Vulnerability Type
    Description
    Evidence
    Data Flow
    Potential Impact
    Confidence
    Validation Needed


Example:

    Finding ID:
        CR-001

    Source:
        Semgrep + Manual Review

    File:
        src/api/invoices.ts

    Line:
        142

    Type:
        Authorization

    Evidence:
        Invoice ID is retrieved from the request
        and queried without an ownership constraint.

    Validation Needed:
        Verify cross-user object access through the API.


# 55. Evidence Requirements

Strong candidate evidence should include:

    File
    Line
    Relevant Code
    Data Flow
    Security Context


Avoid:

    Large unrelated code blocks.


Keep evidence minimal and relevant.


# 56. Confidence

Use confidence to describe:

    Strength of Evidence


Suggested interpretation:

    High
        Clear vulnerable data flow or security-control failure.

    Medium
        Strong indication but important runtime/context assumption remains.

    Low
        Possible issue requiring substantial validation.


Confidence is not severity.


# 57. False Positive Analysis

Before forwarding a candidate finding, check for:

    Sanitization
    Validation
    Authorization Middleware
    Framework Protection
    Safe Wrapper
    Allowlist
    Security Boundary
    Runtime Configuration


Do not stop at the suspicious line.

Trace the relevant control flow.


# 58. Framework-Aware Analysis

Understand framework security mechanisms before reporting.

Examples:

    Django
    Flask
    FastAPI
    Spring
    Laravel
    Rails
    Express
    NestJS
    Next.js
    ASP.NET
    React


A framework may provide:

    CSRF Protection
    Output Encoding
    Authentication Middleware
    ORM Parameterization
    Security Headers


Do not assume protections exist.

Verify their actual usage.


# 59. Security Control Verification

For each suspected vulnerability:

    Identify Expected Control
        |
        v
    Locate Control
        |
        v
    Verify Execution Path
        |
        v
    Verify Coverage
        |
        v
    Determine Bypass Possibility


This prevents:

    "Missing in this function"

from automatically becoming:

    "Missing everywhere."


# 60. Code Review vs Runtime Validation

Use code review to establish:

    Potential Vulnerability
    Root Cause
    Security Control Failure


Use runtime testing to establish:

    Actual Exploitability
    Runtime Impact
    Security Boundary Failure


When both are available:

    Code Evidence
        +
    Runtime Evidence


provides stronger validation.


# 61. Cross-Agent Handoff

Candidate findings are passed to:

    .claude/agents/vulnerability-analyst.md


The handoff should include:

    Scope
    Finding ID
    Code Location
    Vulnerability Type
    Evidence
    Data Flow
    Potential Impact
    Confidence
    Validation Recommendation


Do not pass vague statements such as:

    "This code looks insecure."


# 62. Vulnerability Analyst Responsibility

The Vulnerability Analyst decides:

    Validated
    False Positive
    Duplicate
    Inconclusive
    Not Exploitable


The Code Review Skill must respect that decision.


# 63. Reporting Handoff

Only validated findings should normally reach:

    .claude/agents/report-generator.md


Flow:

    Code Review
        |
        v
    Candidate Finding
        |
        v
    Vulnerability Analyst
        |
        v
    Validated Finding
        |
        v
    Report Generator


# 64. Severity

Do not independently establish final severity.

If an initial severity estimate is useful:

    Mark it:

        Initial Severity


Final severity must follow:

    .claude/rules/severity-model.md


and the Vulnerability Analyst's validated assessment.


# 65. Attack Chains

Code review may reveal prerequisites for an attack chain.

Example:

    SSRF
        +
    Internal Service Exposure
        +
    Weak Authorization
        |
        v
    Privileged Access


Do not declare the complete chain unless the relevant components are validated.


# 66. Multi-Tool Correlation

If:

    Semgrep

and:

    Manual Review


identify the same issue:

    Correlate evidence.


If:

    Semgrep

identifies an issue and:

    Dynamic Testing


confirms exploitability:

    Forward both evidence sources.


The Vulnerability Analyst should consolidate them into one finding when appropriate.


# 67. Review Prioritization

Prioritize code review by:

    Authentication
    Authorization
    Sensitive Data
    External Input
    Privileged Operations
    Network Requests
    File Operations
    Database Operations
    Cryptography
    Administrative Functions


Do not spend equal time on every file.


# 68. High-Risk Files

Prioritize:

    auth/
    security/
    middleware/
    controllers/
    routes/
    services/
    admin/
    payments/
    permissions/
    users/
    uploads/
    integrations/


Actual project structure takes precedence.


# 69. Review Depth

Use progressive depth:

    Level 1:
        Repository Reconnaissance

    Level 2:
        Static Analysis

    Level 3:
        Security-Sensitive Code Review

    Level 4:
        Data-Flow Analysis

    Level 5:
        Cross-Component Reasoning

    Level 6:
        Runtime Validation


The Orchestrator decides how deep the engagement needs to go.


# 70. Review Efficiency

Do not:

    Read every file line-by-line without prioritization.

Do:

    Map architecture.

    Identify attack surfaces.

    Identify security boundaries.

    Search for high-risk sinks.

    Trace relevant data flows.

    Validate security controls.


The objective is:

    High Security Coverage per Unit of Analysis.


# 71. Safe Analysis

Code review itself should not:

    Modify Production Data
    Execute Destructive Commands
    Expose Secrets
    Disable Security Controls


When runtime execution is necessary:

    Follow the Security Agent's authorized workflow.


# 72. Secrets Handling

If a secret is discovered:

    Do not reproduce the complete secret in reports.

Use:

    [REDACTED]


Record enough context to establish:

    Secret Type
    Location
    Exposure


If the secret appears active:

    Escalate through the appropriate security workflow.


# 73. Review Limitations

Record limitations such as:

    Partial Repository
    Missing Dependencies
    Missing Build Configuration
    Unavailable Environment
    Obfuscated Code
    Generated Code
    Missing Runtime Context
    Incomplete Authentication Context


Limitations must influence confidence where appropriate.


# 74. Review Completion Criteria

Code review is complete when:

    [ ] Scope defined
    [ ] Repository structure understood
    [ ] Languages identified
    [ ] Frameworks identified
    [ ] Entry points identified
    [ ] Trust boundaries mapped
    [ ] Security-sensitive components reviewed
    [ ] Static analysis performed where appropriate
    [ ] Candidate findings normalized
    [ ] False positives considered
    [ ] Findings prepared for validation
    [ ] Limitations recorded


Completion does not mean:

    "No vulnerabilities exist."


It means:

    "The defined code-review scope was analyzed according to the selected depth."


# 75. Quality Gate

Before handing findings to the Vulnerability Analyst:

    [ ] Finding is in scope.
    [ ] Evidence is concrete.
    [ ] File and location are known.
    [ ] Relevant data flow is understood.
    [ ] Security control was considered.
    [ ] Framework protections were checked.
    [ ] False-positive possibilities were considered.
    [ ] Impact is plausible and explained.
    [ ] Confidence is justified.
    [ ] Validation steps are clear.
    [ ] Secrets are redacted.


# 76. Final Principle

The Code Review Skill exists to answer:

    "What security weaknesses may exist in the code,
     and what evidence supports that conclusion?"


It does not answer:

    "How many scanner alerts can we produce?"


The correct pipeline is:

    Understand
        ->
    Detect
        ->
    Trace
        ->
    Analyze
        ->
    Challenge
        ->
    Validate
        ->
    Report


The framework's responsibility boundaries remain:

    Code Review Agent
        ->
    Source-Code Security Analysis

    Semgrep
        ->
    Pattern Detection

    Security Agent
        ->
    Runtime Security Testing

    QA Agent
        ->
    Functional / E2E Testing

    Vulnerability Analyst
        ->
    Finding Truth and Validation

    Report Generator
        ->
    Professional Security Reporting

    Security Orchestrator
        ->
    Overall Control and Coordination


The ultimate objective is not to produce more findings.

The objective is to produce:

    Fewer False Positives
        +
    Stronger Evidence
        +
    Better Root-Cause Understanding
        +
    Actionable Remediation
        +
    Reliable Security Decisions


A code review is successful when an engineer can take a validated finding and answer:

    Where is the problem?

    Why does it happen?

    Can it actually be exploited?

    What is the impact?

    How should it be fixed?

    How can the fix be verified?


That is the standard this skill must maintain.