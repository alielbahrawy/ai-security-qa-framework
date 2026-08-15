# Security Patterns Knowledge Base

## Purpose

This document is a practical security knowledge layer for the AI Security & QA Engineering Framework.

It provides reusable security patterns that help:

    Security Orchestrator
    Security Agent
    Code Review Agent
    QA Agent
    Vulnerability Analyst

understand common application security behaviors,
identify relevant testing opportunities,
correlate evidence,
and reason about root causes.

This file is a knowledge source.

It is NOT:

    A workflow
    A tool-selection policy
    A severity calculator
    A replacement for evidence
    A substitute for validation


The framework uses:

    .claude/rules/workflow.md

for:

    WHEN to perform an action


    .claude/rules/tool-selection.md

for:

    WHICH tool to use


    .claude/rules/severity-model.md

for:

    HOW validated risk is classified


    .claude/knowledge/security-patterns.md

for:

    WHAT security patterns and defensive concepts
    should be recognized


# 1. Core Security Reasoning Model

The framework should reason about applications
through security-relevant patterns.

The general model is:

    Input
      |
      v
    Processing
      |
      v
    Trust Boundary
      |
      v
    Authorization
      |
      v
    Sensitive Operation
      |
      v
    Output


Security weaknesses frequently occur when
one of these transitions lacks an appropriate control.


# 2. Trust Boundaries

A trust boundary exists whenever data or authority
moves between components with different trust levels.

Common boundaries:

    Browser -> Backend
    Frontend -> API
    API -> Database
    User -> Application
    Application -> Third Party
    Container -> Host
    Service -> Service
    External Network -> Internal Network
    CI/CD -> Deployment Environment


When analyzing an application, identify:

    Who controls the input?
    Who authenticates it?
    Who authorizes it?
    Who processes it?
    Where is it stored?
    Where does it cross a trust boundary?


# 3. Input Validation Pattern

User-controlled input should not automatically
be considered trusted.

Typical sources:

    HTTP Parameters
    Query Parameters
    Request Bodies
    Headers
    Cookies
    File Names
    Uploaded Files
    JSON
    GraphQL Variables
    Form Data
    WebSocket Messages
    CLI Arguments
    Environment Inputs


Security analysis should determine:

    Is the input expected?
    Is its type validated?
    Is its length constrained?
    Is its structure validated?
    Is it normalized?
    Is it safely consumed?
    Does it reach a sensitive operation?


# 4. Output Encoding Pattern

Data should be encoded according to
the context in which it is rendered.

Relevant contexts:

    HTML
    JavaScript
    CSS
    URL
    JSON
    SQL
    Shell
    Template


A value safely encoded for one context
may not be safe for another.

Therefore:

    Context-Specific Encoding


is preferred over generic escaping.


# 5. Authentication Pattern

Authentication answers:

    "Who are you?"


A secure authentication flow generally includes:

    Identity Verification
    Credential Validation
    Session Establishment
    Session Protection
    Session Expiration
    Account Recovery
    Appropriate Error Handling


Security analysis should inspect:

    Login
    Logout
    Password Reset
    MFA
    Session Handling
    Remember-Me
    Account Recovery


# 6. Authorization Pattern

Authorization answers:

    "Are you allowed to do this?"


Authentication does not imply authorization.

A secure architecture should enforce:

    Authentication
        +
    Authorization


at the server-side resource boundary.


# 7. Server-Side Authorization

Authorization must not depend solely
on frontend restrictions.

Unsafe pattern:

    Frontend hides button
        |
        v
    Backend accepts request anyway


Preferred pattern:

    Request
      |
      v
    Authentication
      |
      v
    Authorization
      |
      v
    Resource Access


The backend must enforce the decision.


# 8. Object-Level Authorization

Whenever a request references an object:

    /users/123
    /orders/456
    /documents/789


the system should determine whether
the authenticated principal may access
that specific object.

Pattern:

    User Identity
        +
    Requested Object
        +
    Authorization Policy
        |
        v
    Allow / Deny


Do not assume that knowing an object identifier
grants access.


# 9. Function-Level Authorization

Different roles may have access
to different operations.

Example:

    User
        ->
    Read Own Profile


    Admin
        ->
    Read Any Profile


The backend must enforce role or permission
requirements for privileged functions.


# 10. Session Management

A secure session should have:

    Strong Session Identifier
    Appropriate Expiration
    Secure Transport
    Appropriate Cookie Flags
    Session Invalidation
    Rotation When Necessary


Review:

    Login
    Logout
    Session Renewal
    Password Change
    Privilege Change


# 11. Credential Handling

Credentials should be treated as
highly sensitive security material.

Examples:

    Passwords
    API Keys
    Access Tokens
    Refresh Tokens
    Private Keys
    Client Secrets


Security review should verify:

    Secure Storage
    Limited Exposure
    Appropriate Rotation
    Access Control
    Logging Hygiene


Never intentionally expose secrets
during testing when unnecessary.


# 12. Secret Management Pattern

Preferred pattern:

    Application
        |
        v
    Secret Management System
        |
        v
    Runtime Secret


Avoid:

    Hardcoded Secret
        |
        v
    Source Repository
        |
        v
    Build Artifact


Potential secret locations include:

    Source Code
    Configuration
    Environment Files
    CI/CD Variables
    Logs
    Documentation
    Client Bundles


# 13. Sensitive Data Exposure

Sensitive information should only be
available to authorized parties.

Examples:

    Personal Data
    Authentication Data
    Financial Data
    Internal Configuration
    Security Tokens
    Private Business Data


Review:

    API Responses
    Logs
    Errors
    Frontend Bundles
    Debug Endpoints
    Metadata


# 14. Error Handling

Errors should provide enough information
for legitimate debugging without unnecessarily
revealing sensitive implementation details.

Potential leakage:

    Stack Traces
    File Paths
    Database Details
    Internal IP Addresses
    Credentials
    Tokens
    Framework Internals


Development diagnostics should not
automatically become public production output.


# 15. Logging Pattern

Security-relevant events should be logged
appropriately.

Potential events:

    Authentication Failure
    Privilege Changes
    Sensitive Operations
    Administrative Actions
    Security Events


Logs should avoid unnecessary sensitive data.

Do not log:

    Passwords
    Session Secrets
    API Keys
    Private Keys


unless there is a deliberate,
secure operational requirement.


# 16. Audit Trail Pattern

Sensitive actions should be attributable.

A useful audit event may include:

    Actor
    Action
    Resource
    Result
    Timestamp
    Context


The exact information depends
on the application and privacy requirements.


# 17. Injection Pattern

Injection risk occurs when untrusted input
changes the meaning of an interpreter command.

Potential interpreters include:

    SQL
    NoSQL
    OS Shell
    LDAP
    XPath
    Template Engines
    Expression Languages
    Query Languages


General pattern:

    Untrusted Input
        |
        v
    Interpreter
        |
        v
    Changed Meaning


Preferred defenses include:

    Parameterization
    Structured APIs
    Safe Query Builders
    Context-Aware Encoding
    Input Validation


# 18. SQL Query Pattern

Preferred:

    Application
        |
        v
    Parameterized Query
        |
        v
    Database


Risky pattern:

    Application
        |
        v
    String Construction
        |
        v
    Database


Code review should identify
whether user-controlled values
can influence query structure.


# 19. Command Execution Pattern

Applications sometimes invoke
operating system commands.

Security analysis should identify:

    Command Construction
    Input Source
    Sanitization
    Argument Separation
    Privilege Level


Preferred design:

    Avoid Shell When Possible
        |
        v
    Structured Process Invocation


If shell execution is required:

    Strict Input Controls
    Safe Argument Handling
    Least Privilege


# 20. Template Injection Pattern

Template engines may interpret
data as executable template syntax.

Pattern:

    User Input
        |
        v
    Template Construction
        |
        v
    Template Engine


Review whether user-controlled content
can influence template structure rather than
only template data.


# 21. Cross-Site Scripting Pattern

XSS risk generally involves:

    Attacker-Controlled Input
        |
        v
    Browser Execution Context


Relevant forms include:

    Stored
    Reflected
    DOM-Based


Review:

    Input Handling
    Output Context
    DOM Manipulation
    Template Rendering


# 22. Browser Security Boundary

The browser should be treated
as an untrusted execution environment.

Never assume:

    Hidden UI
    Disabled Button
    Client-Side Validation
    Client-Side Role Check


provides sufficient security.

Security controls involving
authorization or sensitive operations
belong on trusted server-side boundaries.


# 23. Cross-Site Request Forgery Pattern

State-changing requests may require
protection against unwanted cross-site actions.

Relevant controls may include:

    CSRF Tokens
    SameSite Cookies
    Origin Validation
    Appropriate Request Validation


Applicability depends on:

    Authentication Mechanism
    Browser Behavior
    Request Architecture


# 24. CORS Pattern

CORS controls browser-origin access.

Review:

    Allowed Origins
    Credentials
    Methods
    Headers


Important distinction:

    CORS is a browser policy mechanism.

It is not a replacement for:

    Authentication
    Authorization


# 25. File Upload Pattern

File uploads create multiple
security boundaries.

Review:

    File Type Validation
    Content Validation
    File Size
    Storage Location
    File Name Handling
    Execution Permissions
    Access Control
    Malware Scanning


Never trust:

    Client-Provided MIME Type


alone.


# 26. Path Handling Pattern

User-controlled paths can affect
filesystem operations.

Review:

    Path Construction
    Canonicalization
    Allowed Directories
    File Permissions


Preferred model:

    User Input
        |
        v
    Validated Identifier
        |
        v
    Controlled Resource


rather than:

    User Input
        |
        v
    Arbitrary Filesystem Path


# 27. SSRF Pattern

Server-Side Request Forgery occurs when
a server makes network requests based
on attacker-controlled input.

Pattern:

    User Input
        |
        v
    Server-Side HTTP Client
        |
        v
    Target Resource


Review:

    URL Validation
    Destination Restrictions
    Protocol Restrictions
    Redirect Handling
    Network Segmentation


# 28. Redirect Pattern

Redirect destinations influenced
by untrusted input should be validated.

Preferred:

    Allowlisted Destinations


rather than:

    Arbitrary User-Supplied URL


# 29. Deserialization Pattern

Deserialization converts serialized
data into application objects.

Review:

    Input Source
    Serialization Format
    Trusted / Untrusted Boundary
    Object Construction
    Gadget Exposure


Prefer safe formats and
explicit schemas where possible.


# 30. API Security Pattern

APIs should enforce security
at every relevant resource boundary.

Review:

    Authentication
    Authorization
    Input Validation
    Rate Limiting
    Object Access
    Function Access
    Error Handling
    Data Exposure


Do not assume:

    Internal API = Trusted API


# 31. API Schema Pattern

Schemas should constrain expected data.

Review:

    Types
    Required Fields
    Length
    Enumerations
    Nested Objects
    Unknown Fields


Schema validation should complement
business authorization.


# 32. Rate Limiting Pattern

Sensitive operations may require
rate limiting.

Examples:

    Login
    Password Reset
    OTP
    Search
    Expensive Queries
    Resource Creation


Rate limiting should be based on
actual abuse scenarios and architecture.


# 33. Business Logic Pattern

Not every vulnerability is
a technical injection flaw.

Business logic weaknesses occur when
valid operations can be combined
in unintended ways.

Examples:

    Price Manipulation
    Workflow Bypass
    Coupon Abuse
    Duplicate Transactions
    Race Conditions
    Privilege Workflow Abuse


Testing should model:

    Intended Business Flow

against:

    Actual Allowed State Transitions


# 34. State Machine Pattern

Many applications behave as state machines.

Example:

    CREATED
       |
       v
    PAID
       |
       v
    SHIPPED
       |
       v
    COMPLETED


Security analysis should verify
that transitions are authorized.

Example:

    CREATED
       |
       X
    COMPLETED


may represent an invalid transition.


# 35. Race Condition Pattern

Race conditions occur when
security-relevant state changes
are not synchronized correctly.

Potential targets:

    Payments
    Inventory
    Permissions
    Token Usage
    Account State
    Resource Creation


Review:

    Check-Then-Act
    Concurrent Requests
    Transaction Boundaries
    Locking


# 36. Privilege Escalation Pattern

Privilege escalation occurs when
a principal gains authority beyond
its intended permission.

Pattern:

    Low Privilege
        |
        v
    Missing Authorization Control
        |
        v
    Higher Privilege


Review both:

    Horizontal Escalation

and:

    Vertical Escalation


# 37. Horizontal Access Pattern

Horizontal access means:

    User A
        |
        X
    User B's Resource


Authorization should verify ownership
or explicit permission.


# 38. Vertical Access Pattern

Vertical escalation means:

    Normal User
        |
        X
    Administrative Operation


Role checks must occur
at the privileged operation boundary.


# 39. Least Privilege Pattern

Every component should receive
only the permissions it requires.

Apply to:

    Users
    Services
    Containers
    Databases
    Cloud Roles
    CI/CD
    Applications


Least privilege reduces blast radius.


# 40. Service-to-Service Security

Internal services should not
automatically trust each other.

Review:

    Service Identity
    Authentication
    Authorization
    Network Boundaries
    Secret Management


Internal network location
does not equal authorization.


# 41. Third-Party Integration Pattern

Third-party integrations introduce
external trust dependencies.

Review:

    Credentials
    Data Sharing
    Webhooks
    Callback Validation
    OAuth
    API Permissions
    Failure Handling


Treat third-party responses
as external data unless trusted
by explicit design.


# 42. Webhook Security Pattern

Webhooks should authenticate
the sender and validate integrity.

Common controls:

    Signature Validation
    Timestamp Validation
    Replay Protection
    Schema Validation
    Idempotency


Do not trust webhook payloads
simply because they arrive
from a known endpoint.


# 43. OAuth Pattern

OAuth implementations should be reviewed
according to the actual flow in use.

Review:

    Client Identity
    Redirect URI
    State
    PKCE where applicable
    Token Handling
    Scope
    Token Storage


Avoid assuming:

    "Uses OAuth" = secure


Implementation matters.


# 44. Token Security Pattern

Tokens should have:

    Appropriate Scope
    Appropriate Lifetime
    Secure Storage
    Controlled Issuance
    Revocation Strategy where applicable


Review:

    Access Tokens
    Refresh Tokens
    Reset Tokens
    Verification Tokens


# 45. JWT Pattern

JWT security depends on:

    Signature Validation
    Algorithm Handling
    Key Management
    Claims Validation
    Expiration
    Issuer
    Audience


Never trust claims solely because
they appear inside a token.


# 46. Cryptographic Pattern

Cryptography should be used through
well-established libraries and protocols.

Avoid:

    Custom Cryptographic Algorithms
    Hardcoded Keys
    Weak Hashing
    Insecure Randomness


Review:

    Key Generation
    Key Storage
    Key Rotation
    Algorithm Selection
    Randomness


# 47. Password Storage Pattern

Passwords should not be stored
as plaintext.

Use password hashing schemes
designed for password storage.

Important properties:

    Adaptive Cost
    Salt
    Appropriate Configuration


Do not use ordinary fast hashes
as password storage by themselves.


# 48. Randomness Pattern

Security-sensitive values require
cryptographically secure randomness.

Examples:

    Session IDs
    Reset Tokens
    Verification Tokens
    Temporary Secrets


Do not rely on predictable
general-purpose randomness.


# 49. Transport Security Pattern

Sensitive communication should
use appropriate transport protection.

Review:

    HTTPS
    TLS Configuration
    Certificate Validation
    Secure Cookies


Do not treat encrypted transport
as a replacement for authorization.


# 50. Dependency Security Pattern

Third-party dependencies create
a software supply-chain boundary.

Review:

    Known Vulnerabilities
    Dependency Versions
    Lockfiles
    Package Sources
    Maintainer Trust
    Build Integrity


Dependency findings should be
interpreted in application context.


# 51. Supply Chain Pattern

Security-sensitive build pipelines
should protect:

    Source
    Dependencies
    Build Environment
    Secrets
    Artifacts
    Deployment


Review:

    CI/CD Permissions
    Secret Exposure
    Artifact Integrity
    Dependency Installation


# 52. Container Security Pattern

Containers should follow
least-privilege principles.

Review:

    Base Images
    Package Versions
    User Privileges
    Capabilities
    Secrets
    Mounted Volumes
    Network Access


Do not assume:

    Container = Security Boundary


# 53. Cloud Security Pattern

Cloud resources should follow:

    Least Privilege
    Strong Identity
    Explicit Network Controls
    Secret Management
    Logging
    Resource Isolation


Review:

    IAM
    Storage
    Compute
    Databases
    Serverless
    Network Policies


# 54. Configuration Security Pattern

Security posture can depend heavily
on configuration.

Review:

    Debug Mode
    Development Endpoints
    Default Credentials
    CORS
    Cookies
    TLS
    Error Handling
    Feature Flags


Configuration must be evaluated
in its actual environment.


# 55. Debug Endpoint Pattern

Development-only endpoints
must not unintentionally become
production interfaces.

Potential examples:

    Debug APIs
    Health Diagnostics
    Admin Panels
    Test Routes
    Internal Documentation


Review exposure and authorization.


# 56. Health Check Pattern

Health endpoints should expose
only necessary information.

Avoid unnecessary disclosure of:

    Credentials
    Internal Topology
    Detailed Stack Information
    Sensitive Configuration


# 57. Frontend Secret Pattern

Anything shipped to a browser
should be considered public.

Do not place:

    Private API Keys
    Server Secrets
    Database Credentials
    Signing Keys


inside frontend bundles.


# 58. Client-Side Validation Pattern

Client-side validation improves
user experience but is not sufficient
for security enforcement.

Pattern:

    Client Validation
        +
    Server Validation


not:

    Client Validation
        =
    Security Boundary


# 59. Serialization Pattern

Structured serialization should
use explicit schemas where possible.

Review:

    Type Confusion
    Unexpected Fields
    Recursive Structures
    Size Limits


# 60. Resource Exhaustion Pattern

Applications should protect
resource-intensive operations.

Potential resources:

    CPU
    Memory
    Disk
    Database Connections
    Threads
    Network
    Queue Capacity


Controls may include:

    Limits
    Timeouts
    Quotas
    Rate Limits
    Backpressure


# 61. Timeout Pattern

External operations should
have appropriate timeouts.

Examples:

    HTTP Requests
    Database Queries
    Queue Operations
    File Operations


Unbounded waiting can become
an availability risk.


# 62. Database Access Pattern

Applications should minimize
database privileges.

Preferred:

    Application Role
        |
        v
    Required Database Permissions


not:

    Application
        |
        v
    Full Administrative Database Access


# 63. Transaction Pattern

Security-sensitive state changes
should use appropriate transaction
boundaries.

Review:

    Atomicity
    Consistency
    Isolation
    Concurrency


especially for:

    Payments
    Permissions
    Inventory
    Account Changes


# 64. Data Validation Pattern

Validation should occur
at the appropriate trust boundary.

Recommended:

    External Input
        |
        v
    Schema Validation
        |
        v
    Business Validation
        |
        v
    Sensitive Operation


# 65. Defense-in-Depth Pattern

Critical controls should not depend
on one fragile mechanism.

Example:

    Authentication
        +
    Authorization
        +
    Input Validation
        +
    Least Privilege
        +
    Logging


A single control failure
should not automatically create
complete compromise.


# 66. Fail-Secure Pattern

When security decisions fail,
the system should generally
default toward denying access.

Example:

    Authorization Service Unavailable


should not automatically become:

    Allow Access


unless the architecture explicitly
defines another safe behavior.


# 67. Secure Defaults Pattern

Default configuration should
prefer the safer behavior.

Examples:

    Authentication Required
    Secure Cookies
    Minimal Permissions
    Debug Disabled
    Restricted Origins


# 68. Explicit Trust Pattern

Do not infer trust from:

    Network Location
    Internal IP
    Frontend Origin
    User-Supplied Role
    Client-Side State


Trust should be established
through explicit security controls.


# 69. Defense Against False Positives

A detected pattern is not automatically
a confirmed vulnerability.

The framework must ask:

    Is the input attacker-controlled?
    Does it reach the sensitive operation?
    Is there a security control?
    Is the control effective?
    Is exploitation possible?
    What is the actual impact?


Then:

    Vulnerability Analyst


validates the finding.


# 70. Pattern-to-Evidence Relationship

Knowledge patterns generate
hypotheses.

The process is:

    Security Pattern
        |
        v
    Test Hypothesis
        |
        v
    Tool Execution
        |
        v
    Evidence
        |
        v
    Validation


Knowledge should guide testing,
not fabricate findings.


# 71. Pattern-to-Tool Relationship

Patterns influence tool selection,
but the final tool decision belongs to:

    .claude/rules/tool-selection.md


Example:

    Injection Pattern
        |
        +--> Static Analysis
        |
        +--> Dynamic Testing


The Orchestrator decides
which path is appropriate
based on scope and environment.


# 72. Pattern-to-Agent Relationship

Typical mapping:

    Authentication Pattern
        ->
    Security Agent
    QA Agent


    Authorization Pattern
        ->
    Security Agent
    QA Agent
    Vulnerability Analyst


    Source-Level Injection Pattern
        ->
    Code Review Agent
    Security Agent


    Business Logic Pattern
        ->
    Security Agent
    QA Agent


# 73. Security Pattern Categories

The knowledge base can be grouped into:

    Identity
    Authorization
    Input Handling
    Output Handling
    Injection
    Session Management
    API Security
    Business Logic
    Data Protection
    Cryptography
    Infrastructure
    Cloud
    Supply Chain
    Availability
    Observability


# 74. Pattern Priority

Patterns should be prioritized
according to:

    Application Architecture
    Attack Surface
    Data Sensitivity
    Business Function
    Exposure
    Existing Evidence


Do not blindly test every pattern
against every application.


# 75. Architecture-Aware Testing

The same pattern may have
different relevance depending
on architecture.

Example:

    Server-Side Rendering


may increase relevance of:

    Template Injection


while:

    Pure Static Frontend


may change the analysis significantly.


# 76. Evidence Hierarchy

When interpreting security patterns,
prefer:

    Direct Reproduction
        >
    Strong Runtime Evidence
        >
    Correlated Evidence
        >
    Static Evidence
        >
    Pattern Match Alone


A pattern match alone
is not proof of exploitation.


# 77. Security Pattern Lifecycle

Patterns should follow:

    Recognize
       |
       v
    Hypothesize
       |
       v
    Test
       |
       v
    Collect Evidence
       |
       v
    Validate
       |
       v
    Classify
       |
       v
    Report


# 78. Continuous Improvement

When repeated assessments reveal
new reusable patterns:

    Observe
       |
       v
    Validate
       |
       v
    Document
       |
       v
    Add to Knowledge Base


New knowledge must not override
the authoritative rules without
deliberate review.


# 79. Relationship With Common Vulnerabilities

This file describes:

    Security Patterns


The file:

    .claude/knowledge/common-vulnerabilities.md


should describe:

    Specific vulnerability classes
    Their characteristics
    Detection clues
    Validation considerations


The two files complement each other.


# 80. Relationship With Testing Strategy

The file:

    .claude/knowledge/testing-strategy.md


should describe:

    How to construct practical tests
    across application layers.


This file provides:

    Security Concepts


Testing Strategy provides:

    Testing Approach


# 81. Relationship With Security Audit Skill

The skill:

    .claude/skills/security-audit/SKILL.md


uses this knowledge when
performing security assessments.


The skill determines:

    HOW to execute a security audit


This file provides:

    Security Reasoning Context


# 82. Relationship With Security Agent

The Security Agent should use
this knowledge to:

    Identify relevant attack surfaces
    Generate test hypotheses
    Interpret tool results
    Identify missing controls
    Request additional validation


The Security Agent must still
follow the workflow and tool rules.


# 83. Relationship With Code Review Agent

The Code Review Agent should use
security patterns to recognize:

    Dangerous Data Flows
    Missing Authorization
    Unsafe Input Handling
    Secret Exposure
    Insecure Configuration
    Dangerous API Usage


Static evidence remains subject
to validation.


# 84. Relationship With QA Agent

The QA Agent should use
security patterns to identify
functional workflows with
security implications.

Examples:

    Login
    Password Reset
    Role Changes
    Payments
    File Uploads
    Account Management


Security-sensitive behavior
may be escalated to the
Security Agent.


# 85. Relationship With Vulnerability Analyst

The Vulnerability Analyst uses
security patterns to answer:

    Does the observed behavior
    represent a real security issue?


The Analyst must prioritize:

    Evidence
    Context
    Reproduction
    Impact


over pattern matching alone.


# 86. Relationship With Report Generator

The Report Generator may use
security patterns to explain:

    Root Cause
    Security Principle Violated
    Remediation Strategy


It must not convert
a theoretical pattern into
a confirmed finding without evidence.


# 87. Core Rule

Security patterns are:

    Reasoning Aids


They are not:

    Findings


A pattern becomes a finding only
after sufficient evidence and
validation.


# 88. Final Security Reasoning Loop

The framework should continuously
reason through:

    What is trusted?
        |
        v
    What is controlled by the user?
        |
        v
    Where does the data go?
        |
        v
    What security boundary is crossed?
        |
        v
    What control should exist?
        |
        v
    Does the control exist?
        |
        v
    Is the control effective?
        |
        v
    Can the behavior be reproduced?
        |
        v
    What is the actual impact?


Then:

    Vulnerability Analyst
        |
        v
    Validate
        |
        v
    Severity Model
        |
        v
    Report


# 89. Final Principle

The framework should not ask only:

    "Does this code look dangerous?"


It should ask:

    "What trust boundary exists?"

    "Who controls the input?"

    "What security control should exist?"

    "Where is that control enforced?"

    "Can the behavior be reproduced?"

    "What is the real impact?"


This transforms the framework
from a collection of scanners
into an evidence-driven
security reasoning system.