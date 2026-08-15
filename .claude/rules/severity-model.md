# Severity Model

## 1. Purpose

This document defines the authoritative severity model for the
AI Security & QA Engineering Framework.

The purpose of this model is to ensure that:

    Findings
        |
        v
    Are Validated
        |
        v
    Are Classified Consistently
        |
        v
    Receive Evidence-Based Severity
        |
        v
    Are Reported Correctly


Severity must never be assigned simply because:

    A Tool Reported It
    A Scanner Marked It Critical
    A Vulnerability Name Sounds Dangerous
    A Similar Finding Was Previously Critical


Severity is determined from:

    Validity
    Exploitability
    Impact
    Reachability
    Required Privileges
    Required User Interaction
    Scope
    Business Context
    Evidence Quality


# 2. Authority

This file is the authoritative severity policy.

The:

    Vulnerability Analyst

is responsible for applying this model during finding validation.

The:

    Security Orchestrator

uses severity to determine:

    Validation Priority
    Escalation Priority
    Workflow Priority


The:

    Report Generator

uses the validated severity when generating reports.


The relationship is:

    Security Orchestrator
            |
            v
       Finding Candidate
            |
            v
    Vulnerability Analyst
            |
            v
       Severity Model
            |
            v
    Validated Finding
            |
            v
    Report Generator


# 3. Severity Is Not Detection

A tool detection is not automatically
a security finding.

The framework distinguishes:

    Tool Alert
        |
        v
    Candidate
        |
        v
    Validated Finding
        |
        v
    Severity


Therefore:

    Tool Alert != Vulnerability

and:

    Tool Severity != Final Severity


# 4. Finding Lifecycle

Every security finding should conceptually follow:

    DETECTED
       |
       v
    TRIAGED
       |
       v
    VALIDATED
       |
       v
    CLASSIFIED
       |
       v
    REPORTED
       |
       v
    RETESTED
       |
       v
    CLOSED


A finding must not be reported as confirmed
before sufficient validation has occurred.


# 5. Severity Levels

The framework uses five primary severity levels:

    INFO
    LOW
    MEDIUM
    HIGH
    CRITICAL


These levels represent security risk,
not merely technical inconvenience.


# 6. Severity Overview

| Severity | General Meaning |
|----------|-----------------|
| INFO | Observation with no demonstrated security impact |
| LOW | Limited impact or difficult exploitation |
| MEDIUM | Meaningful security impact requiring realistic conditions |
| HIGH | Serious security impact with practical exploitation |
| CRITICAL | Severe compromise with broad or catastrophic impact |


# 7. INFO

Use INFO when:

    No security vulnerability has been demonstrated.

Examples:

    Security-relevant observation
    Informational configuration detail
    Best-practice recommendation
    Technology fingerprint
    Non-sensitive version disclosure
    Hardening recommendation


INFO should not be used to hide uncertainty.

If a vulnerability is suspected but not validated,
use the appropriate candidate status rather than
artificially assigning INFO.


# 8. LOW

Use LOW when:

    Security impact exists
    but exploitation or impact is limited.


Typical characteristics:

    Low confidentiality impact
    Low integrity impact
    Low availability impact
    Difficult exploitation
    Limited attack surface
    Strong prerequisites
    Low-value target


Examples may include:

    Minor information disclosure
    Weak security header with limited practical impact
    Low-impact configuration weakness
    Non-sensitive metadata exposure


LOW does not mean:

    "Ignore it."


# 9. MEDIUM

Use MEDIUM when:

    A meaningful security weakness exists
    with realistic but constrained impact.


Typical characteristics:

    Moderate confidentiality impact
    Moderate integrity impact
    Moderate availability impact
    Practical exploitation under conditions
    Limited privilege escalation
    Meaningful user or application impact


Examples may include:

    Moderate IDOR
    Limited sensitive information disclosure
    Security control bypass with restricted scope
    Stored XSS with constrained reach
    Moderate authentication weakness


# 10. HIGH

Use HIGH when:

    The vulnerability can cause serious security impact
    and exploitation is reasonably practical.


Typical characteristics:

    Significant data exposure
    Account compromise
    Privilege escalation
    Sensitive operation manipulation
    Significant authorization bypass
    Practical remote exploitation
    High-value asset compromise


Examples may include:

    Authentication bypass
    Significant IDOR / BOLA
    Privilege escalation
    Sensitive data exposure
    Stored XSS with meaningful victim impact
    High-impact injection


# 11. CRITICAL

Use CRITICAL when:

    The vulnerability enables severe compromise
    with broad or catastrophic impact.


Typical characteristics:

    Remote code execution
    Full system compromise
    Administrative compromise
    Complete authentication bypass
    Large-scale sensitive data compromise
    Broad tenant compromise
    Highly practical exploitation
    Severe impact across the application or environment


CRITICAL should be used carefully.

It must be supported by strong evidence.


# 12. Severity Dimensions

Severity should be evaluated across multiple dimensions:

    Exploitability
    Confidentiality
    Integrity
    Availability
    Privilege
    Scope
    User Interaction
    Attack Surface
    Business Impact
    Evidence Quality


No single dimension automatically determines
the final severity.


# 13. Exploitability

Evaluate:

    Can the vulnerability actually be exploited?


Consider:

    Authentication Required
    Privileges Required
    User Interaction
    Attack Complexity
    Network Reachability
    Required Conditions
    Reliability


General interpretation:

    Easy + Reliable + Remote
        ->
    Higher Risk


    Difficult + Fragile + Highly Constrained
        ->
    Lower Risk


# 14. Confidentiality

Evaluate what information
can be accessed.

Levels:

    NONE
    LOW
    MODERATE
    HIGH


Consider:

    Public Information
    Internal Information
    Personal Information
    Credentials
    Tokens
    Secrets
    Financial Information
    Sensitive Business Data


Examples:

    Public metadata
        ->
    NONE / LOW


    User profile data
        ->
    MODERATE


    Passwords / Tokens
        ->
    HIGH


# 15. Integrity

Evaluate what an attacker
can modify.

Examples:

    Non-sensitive preference
        ->
    LOW


    User-owned data
        ->
    MODERATE


    Other users' data
        ->
    HIGH


    Administrative configuration
        ->
    HIGH


    Server-side executable behavior
        ->
    CRITICAL potential


# 16. Availability

Evaluate whether the vulnerability
can disrupt service availability.

Consider:

    Single User
    Single Resource
    Application Component
    Service
    Entire Environment


Examples:

    Single-request failure
        ->
    LOW


    Persistent account disruption
        ->
    MEDIUM


    Service-wide disruption
        ->
    HIGH


    Broad infrastructure compromise
        ->
    CRITICAL potential


# 17. Privileges Required

Consider the privileges needed
to exploit the vulnerability.

Levels:

    NONE
    LOW
    HIGH


General rule:

    No Authentication
        ->
    Higher Exploitability


    Low-Privilege Account
        ->
    Moderate Exploitability


    Administrative Account
        ->
    Lower Exploitability


However:

    Required Privilege

must be evaluated together with:

    Impact


A vulnerability requiring admin access
may still be extremely dangerous if
it enables complete system compromise.


# 18. User Interaction

Evaluate whether exploitation
requires victim interaction.

Possible values:

    NONE
    REQUIRED


Generally:

    No User Interaction
        ->
    Higher Exploitability


But user interaction alone
must not determine severity.


# 19. Attack Complexity

Consider:

    Number of Preconditions
    Timing Requirements
    Race Conditions
    Special Environment
    Exploit Reliability
    Required Knowledge


Levels:

    LOW
    HIGH


LOW complexity generally increases risk.

HIGH complexity may reduce practical exploitability.


# 20. Network Reachability

Consider how the vulnerable component
can be reached.

Typical levels:

    Internet
    External Network
    Internal Network
    Local Host
    Local Process


Internet-reachable vulnerabilities
generally have higher exposure.


However:

    Internal != Safe


An internal vulnerability may still be
HIGH or CRITICAL if compromise impact is severe.


# 21. Attack Surface

Evaluate:

    How exposed is the vulnerable functionality?


Consider:

    Public Endpoint
    Authenticated Endpoint
    Admin Endpoint
    Internal API
    Background Service
    Local Component


A public endpoint generally increases
the likelihood of exploitation.


# 22. Scope

Scope determines how far the attacker
can move beyond the initially compromised component.

Consider:

    Single User
    Single Account
    Single Tenant
    Multiple Tenants
    Application
    Host
    Network
    Organization


Cross-boundary impact should increase
severity when supported by evidence.


# 23. Cross-Tenant Impact

Multi-tenant applications require
special attention.

Example:

    Tenant A
        |
        v
    Unauthorized Access
        |
        v
    Tenant B Data


This should normally be treated
as significantly higher risk than:

    User A
        |
        v
    User A Data


The Vulnerability Analyst must explicitly
consider tenant boundaries.


# 24. Business Impact

Technical severity and business impact
must both be considered.

Consider:

    Financial Loss
    Privacy Impact
    Regulatory Exposure
    Operational Disruption
    Reputation
    Customer Impact
    Intellectual Property
    Safety-Critical Operations


A technically moderate issue
may become HIGH because of business context.


# 25. Asset Criticality

Classify the affected asset:

    LOW
    MEDIUM
    HIGH
    CRITICAL


Examples:

    Marketing Page
        ->
    LOW


    User Account System
        ->
    HIGH


    Payment Infrastructure
        ->
    CRITICAL


    Identity Provider
        ->
    CRITICAL


Asset criticality can increase
the final severity.


# 26. Data Sensitivity

Data should be classified before
determining impact.

Suggested levels:

    PUBLIC
    INTERNAL
    CONFIDENTIAL
    SENSITIVE
    HIGHLY_SENSITIVE


Examples:

    Public documentation
        ->
    PUBLIC


    Internal configuration
        ->
    INTERNAL


    Customer information
        ->
    CONFIDENTIAL


    Authentication tokens
        ->
    SENSITIVE


    Passwords / private keys
        ->
    HIGHLY_SENSITIVE


# 27. Evidence Quality

Severity must account for evidence quality.

Evidence levels:

    E0 — No Evidence
    E1 — Tool Alert
    E2 — Contextual Evidence
    E3 — Reproduced Behavior
    E4 — Confirmed Exploitation
    E5 — Cross-Validated Exploitation


General rule:

    Higher Severity
        +
    Higher Claim
        =
    Stronger Evidence Required


# 28. Evidence Levels

## E0 — No Evidence

Examples:

    Assumption
    Suspicion
    Unverified Claim


Cannot support a confirmed finding.


## E1 — Tool Alert

Examples:

    Semgrep Rule Match
    Scanner Alert
    Automated Detection


This is a candidate.


## E2 — Contextual Evidence

Examples:

    Vulnerable Code Path Identified
    Relevant Endpoint Confirmed
    Configuration Verified
    Data Flow Confirmed


Still requires validation.


## E3 — Reproduced Behavior

The behavior can be reproduced
under controlled conditions.


Example:

    Unauthorized object access reproduced.


## E4 — Confirmed Exploitation

The vulnerability has been demonstrated
to be exploitable.


Example:

    Authorization bypass successfully
    accesses protected resource.


## E5 — Cross-Validated Exploitation

Multiple independent evidence sources
confirm the vulnerability.


Example:

    Semgrep
        +
    Dynamic Reproduction
        +
    Application Behavior


This is the strongest evidence class.


# 29. Evidence vs Severity

Do not confuse:

    Severity

with:

    Confidence


A finding may be:

    HIGH Severity
    MEDIUM Confidence


or:

    MEDIUM Severity
    HIGH Confidence


Both dimensions must be tracked.


# 30. Confidence Model

Use:

    LOW
    MEDIUM
    HIGH


Confidence represents:

    How certain are we that
    the finding is real?


Severity represents:

    How damaging is it if real?


Therefore:

    Severity != Confidence


# 31. Recommended Confidence Rules

LOW confidence:

    Weak evidence
    Unclear context
    Unreproduced behavior


MEDIUM confidence:

    Strong contextual evidence
    Partial reproduction
    Consistent behavior


HIGH confidence:

    Reliable reproduction
    Clear impact
    Strong evidence
    Cross-validation where appropriate


# 32. Candidate Finding

A candidate finding should contain:

    Title
    Source Tool
    Detection
    Target
    Evidence
    Affected Component
    Potential Impact
    Validation Status
    Confidence


It should not be presented
as a confirmed vulnerability.


# 33. Validated Finding

A validated finding should contain:

    Title
    Description
    Root Cause
    Attack Vector
    Preconditions
    Reproduction
    Evidence
    Impact
    Severity
    Confidence
    Affected Assets
    Remediation


# 34. Severity Decision Process

The Vulnerability Analyst should follow:

    Candidate
        |
        v
    Validate
        |
        v
    Determine Exploitability
        |
        v
    Determine Impact
        |
        v
    Determine Scope
        |
        v
    Determine Business Impact
        |
        v
    Evaluate Evidence
        |
        v
    Assign Severity
        |
        v
    Assign Confidence


# 35. Severity Decision Matrix

Use the following as general guidance:

| Exploitability | Impact | Typical Severity |
|----------------|--------|------------------|
| Low | Low | LOW |
| Low | Moderate | MEDIUM |
| Low | High | MEDIUM / HIGH |
| Moderate | Low | LOW / MEDIUM |
| Moderate | Moderate | MEDIUM |
| Moderate | High | HIGH |
| High | Low | MEDIUM |
| High | Moderate | HIGH |
| High | High | CRITICAL |


This is guidance.

Context may change the final result.


# 36. Critical Override Conditions

A finding may qualify for CRITICAL
when one or more of the following
are demonstrated:

    Remote Code Execution
    Full Administrative Compromise
    Complete Authentication Bypass
    Broad Tenant Compromise
    Critical Secret Exposure
    Complete Infrastructure Compromise
    Catastrophic Business Impact


The analyst must still verify:

    Exploitability
    Scope
    Evidence


# 37. High Severity Conditions

A finding may qualify for HIGH when
it demonstrates:

    Account Takeover
    Significant Privilege Escalation
    Cross-User Sensitive Data Access
    Significant Authorization Bypass
    Sensitive Credential Exposure
    High-Impact Injection
    Serious Business Logic Abuse


The exact severity depends on context.


# 38. Medium Severity Conditions

Typical examples:

    Limited Authorization Bypass
    Moderate Information Disclosure
    Limited Stored XSS
    Moderate Business Logic Weakness
    Restricted Sensitive Data Exposure
    Security Control Bypass with Constraints


# 39. Low Severity Conditions

Typical examples:

    Low-Sensitivity Information Disclosure
    Minor Hardening Issues
    Weak Non-Critical Security Controls
    Low-Impact Configuration Problems


# 40. Vulnerability Class Does Not Determine Severity

Never use:

    "XSS = HIGH"


or:

    "IDOR = CRITICAL"


Instead:

    Vulnerability Class
        +
    Exploitability
        +
    Impact
        +
    Scope
        +
    Business Context
        =
    Severity


# 41. Examples

## Example 1 — IDOR

Scenario:

    Authenticated User A
    can access User B's public profile.


Impact:

    Low


Severity:

    LOW / MEDIUM


If instead:

    User A accesses User B's private financial data.


Impact:

    High


Severity:

    HIGH


If:

    User A accesses data across thousands
    of tenants.


Severity:

    HIGH / CRITICAL


depending on demonstrated impact.


# 42. Example 2 — SQL Injection

Scenario:

    Injection is detected in a non-sensitive
    search feature but cannot access protected data.


Severity:

    MEDIUM


If exploitation provides:

    Database Read Access


Severity:

    HIGH


If exploitation provides:

    Full Server Compromise


Severity:

    CRITICAL


# 43. Example 3 — XSS

Scenario:

    Reflected XSS requires a victim to manually
    open a crafted URL.


Possible severity:

    LOW / MEDIUM


If stored XSS executes for privileged administrators
and enables account takeover:

    HIGH


Severity is based on impact,
not the label "XSS."


# 44. Example 4 — Authentication Bypass

If bypass provides access to:

    Low-value user account


Possible:

    HIGH


If bypass provides:

    Administrative Access


Possible:

    CRITICAL


# 45. Example 5 — Sensitive Token Exposure

If an exposed token:

    Has no meaningful privileges
    Is short-lived
    Is non-sensitive


Possible:

    LOW / MEDIUM


If it provides:

    Administrative API Access


Possible:

    CRITICAL


# 46. Example 6 — Security Misconfiguration

A missing security header
does not automatically become HIGH.

Evaluate:

    Which Header
    Which Browser Behavior
    Which Attack
    Which Application Context
    Practical Impact


Then classify accordingly.


# 47. Chained Vulnerabilities

Multiple weaknesses may form
an attack chain.

Example:

    Information Disclosure
        |
        v
    Credential Exposure
        |
        v
    Authentication Bypass
        |
        v
    Administrative Access


Each vulnerability may have
an individual severity.

The complete chain may have
a higher overall impact.


# 48. Attack Chain Severity

For attack chains:

    Individual Finding Severity

and:

    Chain Severity


must be tracked separately.


Example:

    Finding A:
        MEDIUM


    Finding B:
        HIGH


    Combined Chain:
        CRITICAL


when the chain demonstrates
critical compromise.


# 49. Chained Evidence

The Vulnerability Analyst must document:

    Initial Access
    Intermediate Step
    Privilege Transition
    Final Impact


The final severity must be based on
the demonstrated chain,
not speculation.


# 50. False Positives

If a candidate is determined
to be invalid:

    Status:
        FALSE_POSITIVE


Do not assign:

    LOW


simply because the finding is invalid.


Invalid findings are not low-severity
vulnerabilities.


# 51. Accepted Risk

If a validated vulnerability is
accepted by the organization:

    Status:
        ACCEPTED_RISK


Severity remains the same.


Acceptance changes:

    Risk Treatment


not:

    Technical Severity


# 52. Mitigated Finding

If a vulnerability has been fixed:

    Status:
        MITIGATED


The original severity remains
for historical reporting.


Retesting determines:

    Fix Effectiveness


# 53. Retesting

After remediation:

    Original Finding
        |
        v
    Retest
        |
        v
    Reproduction Attempt
        |
        v
    Result


Possible outcomes:

    FIXED
    PARTIALLY_FIXED
    NOT_FIXED
    REGRESSED
    INCONCLUSIVE


# 54. Regression

If a previously fixed vulnerability
returns:

    Status:
        REGRESSED


Severity should reference
the original risk unless
the new context changes it.


# 55. Severity Changes

Severity may change when:

    Exploitability Changes
    Impact Changes
    Scope Changes
    Asset Criticality Changes
    Business Context Changes
    New Evidence Appears


Every severity change should
have a reason.


# 56. Severity Rationale

Every MEDIUM, HIGH, or CRITICAL finding
should include a concise rationale.

Example:

    Severity:
        HIGH

    Rationale:
        The vulnerability allows an authenticated
        low-privilege user to access sensitive data
        belonging to other users without additional
        interaction.


# 57. Critical Finding Requirements

Before reporting CRITICAL,
the Vulnerability Analyst should verify:

    Exploitation
    Impact
    Scope
    Reproducibility
    Evidence Quality


Where possible,
use independent evidence.


# 58. High Finding Requirements

Before reporting HIGH,
verify:

    Real Vulnerability
    Meaningful Impact
    Practical Exploitability
    Reliable Evidence


# 59. Medium Finding Requirements

Before reporting MEDIUM,
verify:

    Security Impact Exists
    Exploitation Is Plausible
    Evidence Is Sufficient


# 60. Low Finding Requirements

Before reporting LOW,
verify:

    The issue is real
    There is security relevance
    Impact is limited


# 61. Information Requirements

INFO findings may be reported
without exploitation when they represent
useful security observations.

However:

    Do not inflate informational observations
    into vulnerabilities.


# 62. Severity and Tool Selection

Severity may influence
the depth of validation.

Example:

    LOW candidate
        ->
    Standard validation


    HIGH candidate
        ->
    Deeper validation


    CRITICAL candidate
        ->
    Strongest available validation


Tool selection remains governed by:

    .claude/rules/tool-selection.md


# 63. Severity and Workflow

Workflow sequencing is governed by:

    .claude/rules/workflow.md


Severity may affect:

    Priority
    Escalation
    Validation Depth
    Retest Priority


but does not replace workflow rules.


# 64. Severity and Knowledge

Knowledge resources may provide:

    Vulnerability Patterns
    Typical Impact
    Known Attack Methods
    Testing Strategies


Relevant files:

    .claude/knowledge/security-patterns.md
    .claude/knowledge/common-vulnerabilities.md
    .claude/knowledge/testing-strategy.md


Knowledge supports analysis.

It does not replace evidence.


# 65. Severity and Reporting

The Report Generator must include:

    Severity
    Confidence
    Impact
    Evidence
    Reproduction
    Remediation


when appropriate.


The report must distinguish:

    Confirmed
    Suspected
    Informational


findings.


# 66. Severity Language

Use precise language.

Prefer:

    "The vulnerability allows an authenticated
     attacker to access another user's private data."


Avoid:

    "This is extremely dangerous."


Severity must be demonstrated,
not emotionally described.


# 67. No Alarmism

Do not increase severity
to make a report look more serious.

Do not decrease severity
to make a report look safer.


The framework must remain:

    Evidence-Driven
    Reproducible
    Defensible


# 68. Severity Summary Structure

Every validated finding should conceptually contain:

    Finding:
        <title>

    Severity:
        <INFO|LOW|MEDIUM|HIGH|CRITICAL>

    Confidence:
        <LOW|MEDIUM|HIGH>

    Exploitability:
        <assessment>

    Impact:
        <assessment>

    Scope:
        <assessment>

    Evidence:
        <evidence>

    Rationale:
        <why this severity was selected>


# 69. Minimum Validation Standard

Before assigning a non-informational severity:

    The analyst must have sufficient evidence
    that the issue represents a real security weakness.


A tool alert alone is insufficient
for high-confidence reporting.


# 70. Separation of Responsibilities

The:

    Security Agent

discovers and investigates.


The:

    Code Review Agent

performs source-level analysis.


The:

    QA Agent

validates application behavior.


The:

    Vulnerability Analyst

determines:

    Validity
    Exploitability
    Impact
    Severity
    Confidence


The:

    Report Generator

communicates the validated result.


The:

    Security Orchestrator

coordinates the complete process.


# 71. Final Severity Pipeline

The complete severity pipeline is:

    Tool Output
        |
        v
    Candidate Finding
        |
        v
    Context Analysis
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
    Business Impact
        |
        v
    Evidence Evaluation
        |
        v
    Severity
        +
    Confidence
        |
        v
    Validated Finding
        |
        v
    Report


# 72. Final Principle

The framework must never ask:

    "How severe did the scanner say this is?"


It must ask:

    "What can an attacker actually do,
     under what conditions,
     against which assets,
     with what evidence,
     and what is the resulting impact?"


The final severity must be:

    Evidence-Based
        +
    Context-Aware
        +
    Reproducible
        +
    Defensible


Final rule:

    Detection identifies candidates.

    Validation establishes reality.

    Exploitability establishes practicality.

    Impact establishes damage.

    Scope establishes blast radius.

    Business context establishes organizational risk.

    Evidence establishes confidence.

    The Severity Model converts these factors
    into a consistent security classification.

    No automated tool is allowed to determine
    final severity by itself.