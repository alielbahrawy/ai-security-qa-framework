# Security Guidelines

## Authorized Use Only

This framework is designed for **authorized security assessments only**. Do not test systems without explicit permission from the system owner.

## Scope Restrictions

- **In-scope**: Targets explicitly authorized for testing
- **Out-of-scope**: Production systems, third-party services, payment providers, unrelated databases
- **Discovery does not create authorization**: Finding new assets does not grant testing permission

## Safe Testing Practices

### Low Impact Defaults

- Use rate-limited testing
- Avoid destructive operations
- Preserve data integrity
- Use test accounts only
- Monitor service stability

### Production Precautions

- **Default approach**: Low impact, controlled, rate limited, non-destructive, scope restricted
- **Avoid**: Destructive operations, data deletion, denial of service, account lockout, high-volume testing, uncontrolled exploitation
- **Unless explicitly authorized**: Any destructive or high-impact testing

## Responsible Disclosure

- Report security issues to the appropriate point of contact
- Include sufficient evidence to reproduce findings
- Do not publicly disclose vulnerabilities before remediation is complete
- Follow the engagement's reporting procedures

## What Should Not Be Publicly Posted

- Real target URLs without authorization scrubbing
- Credentials, tokens, or passwords from assessments
- Findings from private assessments without redaction
- Sensitive evidence containing customer data
- Machine-specific configuration details

## Authorization Checklist (Before Testing)

1. Target is owned or explicitly authorized
2. Testing is within defined scope
3. Environment is appropriate for testing
4. Credentials are authorized (if required)
5. Destructive actions are permitted (if applicable)
6. Safety constraints are understood

If authorization is unclear: **Do Not Perform Active Testing**

The framework may continue with safe non-invasive analysis when appropriate.