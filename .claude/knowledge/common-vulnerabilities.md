# Common Vulnerabilities Knowledge Base

## Purpose

This document defines the vulnerability knowledge layer for the AI Security & QA Engineering Framework.

It gives the framework a structured understanding of common vulnerability classes, their typical causes, affected attack surfaces, detection signals, validation requirements, and remediation direction.

This knowledge is consumed by:

    Security Orchestrator
    Security Agent
    Code Review Agent
    QA Agent
    Vulnerability Analyst
    Report Generator

This document describes vulnerability classes.

It does NOT define:

    Tool selection
    Workflow sequencing
    Severity scoring
    Agent delegation
    Report formatting


Those responsibilities belong to:

    .claude/rules/tool-selection.md
    .claude/rules/workflow.md
    .claude/rules/severity-model.md

Operational security assessment procedures belong to:

    .claude/skills/security-audit/SKILL.md

Reusable security concepts belong to:

    .claude/knowledge/security-patterns.md


# 1. Vulnerability Reasoning Model

A vulnerability should be understood as:

    Attacker-Controlled Condition
            |
            v
    Security Control Failure
            |
            v
    Security-Relevant Behavior
            |
            v
    Exploitable Condition
            |
            v
    Security Impact


A code smell alone is not automatically
a vulnerability.

A scanner alert alone is not automatically
a confirmed vulnerability.

A vulnerable dependency alone is not automatically
an exploitable application vulnerability.

The framework must establish context and evidence.


# 2. Vulnerability Classification

Vulnerabilities may affect:

    Confidentiality
    Integrity
    Availability
    Authentication
    Authorization
    Accountability
    Privacy
    Business Logic


A single vulnerability may affect
multiple dimensions.


# 3. Injection Vulnerabilities

## 3.1 SQL Injection

### Description

SQL Injection occurs when attacker-controlled
input influences SQL query structure
without adequate separation between
data and executable query syntax.

### Common Sources

    Query Parameters
    Form Fields
    JSON
    Headers
    Cookies
    Search Functions
    Filtering
    Sorting
    Reporting


### Detection Signals

    Dynamic SQL Construction
    String Concatenation
    Unsafe Query APIs
    Missing Parameterization
    Database Errors
    Unexpected Query Behavior


### Validation Requirements

Determine:

    Is the input attacker-controlled?
    Does it reach a database query?
    Is parameterization present?
    Can query structure be influenced?
    Is there reproducible security impact?


### Preferred Remediation

    Parameterized Queries
    Prepared Statements
    Safe ORM APIs
    Strict Validation


# 4. NoSQL Injection

### Description

NoSQL Injection occurs when untrusted input
can alter the semantics of a NoSQL query.

### Common Targets

    MongoDB
    Document Databases
    Query Objects
    Filter Expressions


### Detection Signals

    User-Controlled Query Objects
    Dynamic Operators
    Unvalidated JSON
    Query Construction From Request Data


### Validation

Establish whether attacker-controlled
data can alter query logic.

### Remediation

    Strict Schemas
    Typed Inputs
    Safe Query Construction
    Operator Restrictions


# 5. OS Command Injection

### Description

Command Injection occurs when untrusted data
influences operating system command execution.

### Common Sources

    File Conversion
    Image Processing
    Network Utilities
    System Administration Features
    Backup Operations
    Developer Tools


### Detection Signals

    Shell Execution
    Dynamic Command Construction
    String Concatenation
    User Input Passed to Commands


### Validation

Determine:

    What command is executed?
    Which arguments are controlled?
    Is a shell involved?
    What privileges does the process have?
    Is execution reproducible?


### Remediation

Prefer:

    Native APIs
    Structured Process Execution
    Argument Separation
    Strict Allowlisting
    Least Privilege


# 6. Server-Side Template Injection

### Description

SSTI occurs when attacker-controlled data
is interpreted as template syntax
by a server-side template engine.

### Common Targets

    Jinja
    Twig
    Freemarker
    Velocity
    Handlebars-like server engines


### Detection Signals

    Dynamic Template Construction
    User-Controlled Template Content
    Template Evaluation APIs


### Validation

Confirm whether attacker-controlled
content can influence template structure
and whether this produces security impact.


### Remediation

    Treat User Data as Data
    Static Templates
    Safe Rendering APIs
    Template Isolation


# 7. Cross-Site Scripting

## 7.1 Reflected XSS

Attacker-controlled input is reflected
into a response and interpreted by
the browser.

Common sources:

    Search
    Error Messages
    Query Parameters
    Form Inputs


## 7.2 Stored XSS

Malicious content is persisted and
later rendered to other users.

Common sources:

    Comments
    Profiles
    Messages
    Posts
    Support Tickets


## 7.3 DOM-Based XSS

Client-side JavaScript processes
untrusted data in a dangerous browser
execution context.

Potential sources:

    URL
    Hash
    postMessage
    Local Storage
    DOM APIs


### Detection Signals

    innerHTML
    outerHTML
    document.write
    Dangerous URL Construction
    Unsafe Template Rendering
    Untrusted HTML


### Validation

Determine:

    Input Source
    Data Flow
    Browser Context
    Output Encoding
    Execution Behavior


### Remediation

    Context-Aware Encoding
    Safe DOM APIs
    Trusted Types where appropriate
    Content Security Policy as defense-in-depth


# 8. Cross-Site Request Forgery

### Description

CSRF occurs when a victim's authenticated
browser is induced to perform an unintended
state-changing action.

### Common Targets

    Account Changes
    Password Changes
    Payments
    Email Changes
    Administrative Actions


### Detection Signals

    Cookie-Based Authentication
    State-Changing Requests
    Missing CSRF Protection
    Weak Origin Validation


### Validation

Establish:

    Authentication Model
    Browser Context
    State-Changing Operation
    Cross-Origin Request Feasibility
    Existing Mitigations


### Remediation

    CSRF Tokens
    SameSite Cookies
    Origin Validation
    Appropriate Request Validation


# 9. Server-Side Request Forgery

### Description

SSRF occurs when a server makes network
requests based on attacker-controlled
destination information.

### Common Targets

    URL Fetchers
    Webhooks
    Image Importers
    PDF Generators
    Proxy Features
    Metadata Services


### Detection Signals

    User-Controlled URLs
    Server-Side HTTP Clients
    URL Preview Features
    Import Functions


### Validation

Determine:

    Can the destination be controlled?
    What protocols are supported?
    Are redirects followed?
    Can internal resources be reached?
    What network privileges does the server have?


### Remediation

    Destination Allowlisting
    Protocol Restrictions
    Network Segmentation
    Redirect Validation
    Egress Filtering


# 10. Path Traversal

### Description

Path Traversal occurs when attacker-controlled
input influences filesystem paths in a way
that allows access outside the intended
directory.

### Common Targets

    File Downloads
    File Reads
    Archive Extraction
    Static File Serving


### Detection Signals

    User-Controlled File Paths
    Path Concatenation
    Weak Canonicalization
    Archive Extraction


### Validation

Determine whether the application can access
resources outside the intended boundary.


### Remediation

    Canonicalize Paths
    Restrict Base Directories
    Use Resource Identifiers
    Reject Unexpected Paths


# 11. Local File Inclusion

### Description

LFI occurs when attacker-controlled input
influences which local resource is loaded
or interpreted by the application.

### Common Targets

    Template Loading
    Dynamic Includes
    Language Runtime Includes


### Validation

Confirm whether arbitrary local resources
can be selected and whether they can affect
application behavior.


# 12. Remote File Inclusion

### Description

RFI occurs when an application loads
remote attacker-controlled resources
through a dynamic inclusion mechanism.

### Risk

Potential impact can include:

    Code Execution
    Data Exposure
    Application Compromise


### Remediation

    Disable Remote Inclusion
    Allowlist Resources
    Avoid Dynamic Includes


# 13. Unrestricted File Upload

### Description

An unrestricted upload vulnerability occurs
when an application allows dangerous files
to be uploaded or processed without adequate
security controls.

### Common Issues

    Executable Content
    Script Uploads
    Oversized Files
    Malicious Content
    Dangerous File Names
    Public Exposure


### Validation

Review:

    File Type Validation
    Content Validation
    Storage
    Execution Permissions
    Retrieval Controls


### Remediation

    Allowlisted Types
    Content Validation
    Safe Storage
    Randomized Names
    Non-Executable Storage
    Size Limits


# 14. Insecure Direct Object Reference

### Description

IDOR occurs when an application exposes
object identifiers without properly enforcing
authorization for the requested object.

Example pattern:

    User A
      |
      v
    /documents/100
      |
      X
    Document belongs to User B


### Validation

Test whether object access is based on:

    Identity
    Ownership
    Explicit Permission


rather than identifier knowledge alone.


# 15. Broken Object-Level Authorization

### Description

BOLA is the API-oriented form of object
authorization failure.

### Common Targets

    REST APIs
    GraphQL APIs
    Mobile APIs
    JSON Endpoints


### Validation

Compare:

    Authenticated Principal
        +
    Requested Resource
        +
    Authorization Policy


# 16. Broken Function-Level Authorization

### Description

BFLA occurs when users can access
functions intended for higher-privileged
roles.

### Common Targets

    Admin APIs
    Management Endpoints
    User Management
    Configuration APIs


### Validation

Compare:

    Required Permission
        vs
    Actual Permission Enforcement


# 17. Privilege Escalation

## 17.1 Horizontal Privilege Escalation

A user gains access to another user's
resources or capabilities.

## 17.2 Vertical Privilege Escalation

A lower-privileged user gains access
to administrative functionality.


### Root Causes

    Missing Authorization
    Trust in Client-Supplied Roles
    Weak Access Control
    Inconsistent Enforcement


# 18. Authentication Bypass

### Description

Authentication bypass occurs when an
application allows protected functionality
without successfully establishing identity.

### Potential Causes

    Missing Authentication Middleware
    Logic Errors
    Trust in Client State
    Alternate Unprotected Endpoints
    Misconfigured Access Controls


### Validation

Do not infer bypass solely from
an accessible endpoint.

Confirm:

    Protected Resource
    Expected Authentication
    Actual Behavior
    Security Impact


# 19. Weak Authentication

Potential weaknesses include:

    Weak Password Policy
    Missing MFA Where Required
    Predictable Recovery
    Credential Enumeration
    Insecure Session Handling
    Weak Rate Limiting


Each must be evaluated in context.


# 20. Account Enumeration

### Description

The application reveals whether
an account exists through observable
differences.

Signals may include:

    Different Error Messages
    Different Response Codes
    Timing Differences
    Different Recovery Behavior


### Impact

Enumeration can assist:

    Credential Attacks
    Social Engineering
    Account Targeting


# 21. Credential Stuffing Exposure

### Description

Applications may be vulnerable to
automated credential reuse attacks
when authentication controls are weak.

Review:

    Rate Limiting
    Detection
    MFA
    Lockout Strategy
    Credential Monitoring


# 22. Session Fixation

### Description

Session fixation occurs when an attacker
can cause a victim to use a session identifier
known to the attacker.

### Validation

Review:

    Session Rotation
    Login Behavior
    Authentication State Changes


# 23. Session Hijacking Exposure

Potential causes:

    Weak Session Tokens
    Token Leakage
    Insecure Cookies
    Transport Issues
    Excessive Token Lifetime


### Review

    Secure
    HttpOnly
    SameSite
    Expiration
    Rotation


# 24. JWT Misconfiguration

Potential weaknesses include:

    Missing Signature Validation
    Weak Algorithm Handling
    Missing Expiration Validation
    Missing Audience Validation
    Missing Issuer Validation
    Excessive Claims Trust


### Important Rule

A JWT payload is not trustworthy
merely because it is structurally valid.


# 25. OAuth Misconfiguration

Potential weaknesses include:

    Weak Redirect URI Validation
    Missing State Validation
    Missing PKCE Where Appropriate
    Excessive Scopes
    Unsafe Token Handling


OAuth security must be evaluated
according to the actual flow.


# 26. Insecure Password Reset

### Potential Issues

    Predictable Tokens
    Reusable Tokens
    Long-Lived Tokens
    Account Enumeration
    Missing Invalidation
    Weak Authorization


### Validation

Test the complete lifecycle:

    Request
      |
      v
    Token
      |
      v
    Verification
      |
      v
    Password Change
      |
      v
    Token Invalidation


# 27. Insecure Account Recovery

Recovery mechanisms should not
provide an alternative path around
normal authentication.

Review:

    Identity Verification
    Token Security
    Expiration
    Replay Protection
    Notification


# 28. Sensitive Data Exposure

Potentially exposed information:

    Passwords
    Tokens
    Personal Data
    Financial Information
    Internal Identifiers
    Private Configuration
    Source Code


Potential locations:

    API Responses
    Logs
    Errors
    Frontend Assets
    Debug Endpoints
    Backups


# 29. Security Misconfiguration

Examples:

    Debug Enabled
    Default Credentials
    Excessive Permissions
    Unnecessary Services
    Open Administrative Interfaces
    Weak CORS
    Verbose Errors


Configuration findings require
environmental context.


# 30. CORS Misconfiguration

Potential problems:

    Overly Broad Origins
    Credentialed Wildcards
    Trusting Arbitrary Origins
    Inconsistent Origin Validation


### Important Rule

CORS does not replace authorization.


# 31. HTTP Security Misconfiguration

Review:

    TLS
    Security Headers
    Cookie Attributes
    Host Validation
    HTTP Methods


Security headers should be evaluated
according to application behavior,
not treated as isolated checkboxes.


# 32. Host Header Injection

### Description

Occurs when applications trust
attacker-controlled Host-related headers
for security-sensitive operations.

Potential impact:

    Password Reset Links
    Cache Behavior
    Routing
    Absolute URL Generation


### Validation

Determine whether the header influences
security-sensitive behavior.


# 33. HTTP Request Smuggling

### Description

Request smuggling can occur when
different components interpret HTTP
request boundaries differently.

Potential components:

    Reverse Proxy
    Load Balancer
    Application Server


### Validation

Requires careful architecture-aware
testing and should not be inferred
from generic proxy behavior.


# 34. HTTP Parameter Pollution

Multiple parameters with the same name
may be interpreted differently by
different components.

Potential impact:

    Authorization Logic
    Validation Bypass
    Routing
    Business Logic


# 35. Open Redirect

### Description

An application redirects users to
attacker-controlled destinations.

### Impact

Potentially enables:

    Phishing
    Trust Abuse
    OAuth Flow Abuse


### Remediation

    Allowlisted Destinations
    Validated Relative Paths


# 36. Clickjacking

### Description

An application may be embedded by
an attacker-controlled page and
trick users into interacting with it.

### Controls

    Content-Security-Policy frame-ancestors
    X-Frame-Options


Applicability depends on
the application's UI behavior.


# 37. Insecure Deserialization

### Description

Unsafe deserialization can cause
unexpected object construction or,
in some environments, code execution.

### Risk Factors

    Untrusted Serialized Data
    Dangerous Object Types
    Dynamic Object Resolution
    Gadget Chains


### Remediation

    Safe Formats
    Explicit Schemas
    Type Restrictions
    Integrity Protection


# 38. XML External Entity

### Description

Unsafe XML processing may allow
external entity resolution.

Potential impact:

    File Disclosure
    SSRF
    Resource Exhaustion


### Validation

Determine:

    XML Parser
    External Entity Configuration
    Input Source
    Parser Behavior


# 39. GraphQL Security Issues

Potential areas:

    Missing Authorization
    Excessive Query Depth
    Excessive Query Complexity
    Introspection Exposure
    Batching Abuse
    Sensitive Field Exposure


GraphQL must be assessed at both:

    Query Layer
    Resolver Authorization Layer


# 40. API Mass Assignment

### Description

Mass assignment occurs when clients
can provide fields that should be
server-controlled.

Potential sensitive fields:

    Role
    Owner
    Status
    Permissions
    Account State


### Validation

Compare:

    Client-Controlled Fields

against:

    Server-Managed Fields


# 41. Excessive Data Exposure

An API may return more information
than the client actually requires.

Review:

    User Objects
    Administrative Objects
    Internal Metadata
    Sensitive Fields


Do not assume unused fields are harmless.


# 42. Unrestricted Resource Consumption

Potential causes:

    Missing Rate Limits
    Unbounded Queries
    Large Uploads
    Expensive Computation
    Deep Recursion
    Excessive Pagination


Potential impact:

    Availability Loss
    Cost Increase
    Resource Exhaustion


# 43. Race Condition

### Description

A race condition occurs when
concurrent operations produce an
unexpected security-relevant state.

Potential targets:

    Payments
    Inventory
    Permissions
    Token Redemption
    Account State


### Validation

Requires evidence involving
concurrent or repeated operations.


# 44. Business Logic Vulnerability

### Description

Business logic vulnerabilities occur
when valid application functions can
be abused to violate intended business rules.

Examples:

    Price Manipulation
    Coupon Reuse
    Workflow Bypass
    Duplicate Redemption
    Unauthorized State Transition


### Validation

Understand the intended workflow
before declaring a violation.


# 45. Payment Logic Vulnerabilities

Potential areas:

    Price Trust
    Currency Handling
    Quantity Manipulation
    Discount Abuse
    Payment State
    Refund Logic
    Duplicate Transactions


Security analysis must distinguish
test environments from real financial systems.


# 46. File and Archive Vulnerabilities

Potential issues:

    Zip Slip
    Archive Bombs
    Path Traversal
    Dangerous Extraction
    Symlink Abuse


### Validation

Inspect extraction behavior
and filesystem boundaries.


# 47. Zip Slip

### Description

Zip Slip occurs when archive entries
can write outside the intended extraction
directory.

### Root Cause

Unsafe handling of archive entry paths.

### Remediation

    Canonicalize
    Validate Destination
    Reject Traversal
    Restrict Extraction Directory


# 48. Prototype Pollution

### Description

Prototype pollution occurs when
attacker-controlled object properties
modify JavaScript object prototypes.

### Potential Sources

    Recursive Merge
    Object Assignment
    Dynamic Property Paths
    Unsafe Parsing


### Impact

Depends heavily on application architecture.


# 49. Dependency Vulnerability

### Description

A dependency may contain a known
security vulnerability.

### Important Rule

A vulnerable dependency version
does not automatically mean the
application is exploitable.

The framework should determine:

    Is the vulnerable component installed?
    Is the affected code path used?
    Is the vulnerable feature reachable?
    Is exploitation possible?


# 50. Supply Chain Vulnerability

Potential issues include:

    Malicious Package
    Compromised Dependency
    Build Script Abuse
    Dependency Confusion
    Artifact Tampering


These findings require strong evidence
because of their potentially broad impact.


# 51. Hardcoded Secrets

Potential locations:

    Source Code
    Configuration
    Scripts
    Tests
    CI Files
    Documentation


Potential secrets:

    API Keys
    Tokens
    Passwords
    Private Keys
    Cloud Credentials


Validation should distinguish
real active secrets from:

    Examples
    Test Values
    Documentation Placeholders


# 52. Insecure Cryptography

Potential issues:

    Weak Algorithms
    Weak Key Sizes
    Hardcoded Keys
    Predictable Randomness
    Improper Key Storage
    Incorrect Cryptographic Usage


The framework should avoid declaring
cryptographic weakness without
understanding the actual security context.


# 53. Weak Password Hashing

Potential problems:

    Plaintext Storage
    Fast General-Purpose Hashes
    Missing Salt
    Weak Cost Configuration


Preferred approach:

    Purpose-Built Password Hashing


# 54. Insecure Randomness

Security-sensitive values should use
cryptographically secure randomness.

Potential targets:

    Tokens
    Session IDs
    Password Reset Codes
    Authentication Challenges


# 55. Privilege Misconfiguration

Potential causes:

    Excessive IAM Permissions
    Broad Service Roles
    Shared Administrative Credentials
    Overprivileged Containers
    Overprivileged Database Accounts


Apply least privilege
at every relevant layer.


# 56. Cloud Storage Exposure

Potential issues:

    Public Buckets
    Excessive Permissions
    Public Objects
    Weak Access Policies


Validation must distinguish:

    Publicly Discoverable

from:

    Publicly Accessible


# 57. Cloud Metadata Exposure

Cloud workloads may expose
metadata services.

Security analysis should consider:

    Server-Side Requests
    Network Restrictions
    Credential Exposure
    Workload Identity


Do not assume metadata access
without evidence.


# 58. Container Security Issues

Potential issues:

    Root Execution
    Excessive Capabilities
    Host Mounts
    Privileged Containers
    Secrets in Images
    Vulnerable Base Images


Impact depends on container runtime
and host configuration.


# 59. Kubernetes Security Issues

Potential areas:

    RBAC
    Service Accounts
    Secrets
    Network Policies
    Pod Security
    Host Mounts
    Privileged Workloads


Architecture context is mandatory.


# 60. CI/CD Security Issues

Potential issues:

    Secret Exposure
    Excessive Runner Permissions
    Unsafe Pull Request Workflows
    Untrusted Code Execution
    Artifact Manipulation


CI/CD should be treated as
a privileged environment.


# 61. Log Injection

### Description

Untrusted input may manipulate
log entries or monitoring systems.

Potential impact:

    Misleading Logs
    Detection Evasion
    Log Parsing Issues


Remediation:

    Structured Logging
    Input Normalization
    Safe Encoding


# 62. Log Sensitive Data Exposure

Potential sensitive data:

    Passwords
    Tokens
    Session IDs
    API Keys
    Personal Data


Security logs should remain
security-useful without becoming
a secret storage mechanism.


# 63. Path-Based Authorization Bypass

Applications may enforce authorization
for one route representation but not another.

Review:

    Alternate Paths
    Encodings
    Case Variations
    Routing Differences
    Direct API Access


The framework should focus on
actual authorization boundaries.


# 64. Method-Based Authorization Bypass

Different HTTP methods may reach
different handlers.

Review:

    GET
    POST
    PUT
    PATCH
    DELETE


Authorization should remain consistent
with the intended operation.


# 65. Content-Type Confusion

Different components may interpret
the same request differently.

Review:

    Content-Type
    Body Parsing
    Request Validation
    Proxy Behavior


# 66. Cache-Related Security Issues

Potential issues include:

    Sensitive Response Caching
    Cache Key Confusion
    Authorization-Aware Caching Failures


Security impact depends strongly
on infrastructure architecture.


# 67. Web Cache Poisoning

Occurs when attacker-controlled
input influences cached content
that is later served to other users.

Validation requires:

    Cache Architecture
    Cache Key
    Response Behavior
    Victim Interaction


# 68. Web Cache Deception

Sensitive dynamic content may become
accessible through caching behavior
that treats a request as static content.

Requires architecture-aware validation.


# 69. WebSocket Security

Review:

    Authentication
    Authorization
    Origin Validation
    Message Validation
    Rate Limiting
    Connection Management


Do not assume the initial HTTP
authentication automatically secures
every message-level action.


# 70. Server-Sent Events Security

Review:

    Authentication
    Authorization
    Data Exposure
    Connection Lifetime
    Resource Consumption


# 71. Mobile API Security

Mobile applications should be treated
as clients operating in an untrusted
environment.

Review:

    API Authorization
    Token Handling
    Certificate Validation
    Sensitive Data Storage
    Client-Controlled Parameters


Do not trust:

    Mobile App Logic
    Hidden API Keys
    Client-Side Roles


# 72. Client-Side Secret Exposure

Anything recoverable from a client
should be considered potentially public.

This includes:

    JavaScript
    Mobile Binaries
    Configuration
    Embedded Credentials


# 73. Sensitive Information in Source Maps

Source maps may expose:

    Source Code
    Internal Paths
    Debug Information
    Application Structure


Security impact depends on what
the exposed material contains.


# 74. Debug Information Exposure

Potential locations:

    Stack Traces
    Debug Pages
    Development APIs
    Error Responses
    Build Artifacts


Validate whether the information
creates meaningful security impact.


# 75. Insecure Defaults

Examples:

    Public Access
    Debug Enabled
    Weak Credentials
    Broad Permissions
    Insecure Cookies


Defaults should be evaluated
in actual deployment context.


# 76. Denial of Service

Potential classes:

    Algorithmic Complexity
    Memory Exhaustion
    CPU Exhaustion
    Connection Exhaustion
    Storage Exhaustion
    Application-Level Flooding


Validation must be controlled
and must avoid unnecessary disruption.


# 77. Regular Expression Denial of Service

ReDoS may occur when attacker-controlled
input causes catastrophic regex behavior.

Review:

    Complex Expressions
    Nested Quantifiers
    Unbounded Input


Validation should prefer safe,
bounded testing rather than destructive load.


# 78. Resource Leak

Potential leaks include:

    File Descriptors
    Connections
    Memory
    Threads
    Temporary Files


Security impact may become
availability-related under repetition.


# 79. Integer and Numeric Validation Issues

Potential problems:

    Integer Overflow
    Underflow
    Negative Values
    Precision Errors
    Quantity Manipulation


Especially relevant to:

    Payments
    Inventory
    Limits
    Pagination
    Resource Allocation


# 80. Type Confusion

Applications may behave unexpectedly
when input types differ from what
the security logic expects.

Examples:

    String vs Integer
    Boolean vs String
    Array vs Object
    Null vs Missing Field


Schema validation can reduce risk.


# 81. Business Rule Bypass

A valid operation may become invalid
when performed in an unintended order.

Review:

    Workflow State
    Preconditions
    Postconditions
    Role Requirements


# 82. Missing Security Control

A missing control is meaningful only
when the control is actually required
by the security boundary.

Examples:

    Missing Authorization
    Missing Validation
    Missing Rate Limiting
    Missing Integrity Check


Context determines whether absence
is a vulnerability.


# 83. Vulnerability Correlation

Multiple weak signals may describe
one underlying vulnerability.

Example:

    Missing Authorization
        +
    Predictable Object IDs
        +
    Cross-User Access
        |
        v
    One Authorization Vulnerability


The Vulnerability Analyst should
deduplicate correlated evidence.


# 84. Duplicate Finding Prevention

Do not report the same root cause
as multiple unrelated vulnerabilities
unless the impacts or affected controls
are materially different.


# 85. False Positive Controls

Potential false positives include:

    Dead Code
    Unreachable Code
    Test Fixtures
    Example Secrets
    Sanitized Inputs
    Protected Endpoints
    Non-Exploitable Dependency Paths


Static findings require contextual review.


# 86. Evidence Requirements

A vulnerability should preferably include:

    Affected Component
    Attack Surface
    Input Source
    Security Boundary
    Failed Control
    Reproduction Evidence
    Impact
    Remediation


The stronger the claim,
the stronger the evidence required.


# 87. Vulnerability Validation Lifecycle

The standard reasoning flow is:

    Detection
        |
        v
    Triage
        |
        v
    Reproduction
        |
        v
    Root Cause Analysis
        |
        v
    Impact Analysis
        |
        v
    Validation
        |
        v
    Severity Classification
        |
        v
    Reporting


# 88. Detection vs Confirmation

Detection means:

    "There may be a vulnerability."


Confirmation means:

    "Evidence demonstrates that
     the security boundary is violated."


The framework must preserve
this distinction.


# 89. Tool Evidence

Tool output may include:

    Static Findings
    Dynamic Findings
    Test Failures
    HTTP Responses
    Logs
    Stack Traces
    Dependency Reports


Tool output is evidence,
not automatically a final conclusion.


# 90. Multi-Agent Correlation

Typical flow:

    Security Agent
        |
        v
    Candidate Finding


    Code Review Agent
        |
        v
    Static Evidence


    QA Agent
        |
        v
    Behavioral Evidence


    Vulnerability Analyst
        |
        v
    Correlated Finding


    Report Generator
        |
        v
    Final Report


# 91. Severity Independence

This knowledge base does not assign
final severity.

Severity belongs to:

    .claude/rules/severity-model.md


The Vulnerability Analyst supplies
validated impact information.

The severity model converts that
information into a consistent classification.


# 92. Remediation Independence

This knowledge base provides
general remediation direction.

Project-specific remediation must
consider:

    Architecture
    Framework
    Language
    Deployment
    Business Requirements


# 93. Safe Testing Principle

Security validation should remain
within the authorized assessment scope.

The framework must avoid:

    Destructive Actions
    Unnecessary Data Modification
    Uncontrolled Denial of Service
    Real-World Financial Transactions
    Credential Abuse Outside Scope


Testing should maximize evidence
while minimizing operational risk.


# 94. Authorization Boundary

Before active testing, the framework
must understand:

    Target
    Scope
    Allowed Actions
    Restrictions


The Security Orchestrator controls
workflow according to:

    .claude/rules/workflow.md


# 95. Knowledge Base Usage Rule

Agents should use this document
to generate and evaluate hypotheses.

They should NOT:

    Treat every listed vulnerability
    as present


They should:

    Identify
    Test
    Collect Evidence
    Validate
    Correlate
    Classify


# 96. Relationship With Security Patterns

The security pattern knowledge describes:

    Security concepts
    Trust boundaries
    Controls
    Data flows
    Defensive principles


This vulnerability knowledge describes:

    Specific vulnerability classes
    Common root causes
    Detection signals
    Validation requirements


Therefore:

    Security Pattern
        |
        v
    Vulnerability Hypothesis
        |
        v
    Testing
        |
        v
    Evidence
        |
        v
    Validation


# 97. Relationship With Testing Strategy

Testing Strategy determines
how practical tests are designed.

This knowledge base determines
what vulnerability class the test
is attempting to evaluate.


# 98. Relationship With Reporting

The Report Generator should use
validated vulnerability information
to produce findings containing:

    Title
    Description
    Evidence
    Impact
    Affected Component
    Severity
    Remediation


The Report Generator must not
invent missing evidence.


# 99. Relationship With Orchestration

The Security Orchestrator should
not manually perform every
vulnerability analysis.

Its role is to:

    Understand Scope
        |
        v
    Identify Relevant Domains
        |
        v
    Delegate Work
        |
        v
    Collect Results
        |
        v
    Trigger Validation
        |
        v
    Request Reporting


The vulnerability knowledge
supports the reasoning behind
those decisions.


# 100. Final Framework Principle

The framework is not designed
to produce the largest possible
number of findings.

It is designed to produce
the smallest set of:

    Accurate
    Reproducible
    Evidence-Based
    Context-Aware
    Actionable


security findings.

The final trust chain is:

    Knowledge
        |
        v
    Hypothesis
        |
        v
    Tool
        |
        v
    Evidence
        |
        v
    Vulnerability Analyst
        |
        v
    Validated Finding
        |
        v
    Severity Model
        |
        v
    Report Generator
        |
        v
    Professional Security Report