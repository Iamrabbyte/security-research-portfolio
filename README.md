# Security Research Portfolio

This repository documents my hands-on web application and API security research.

## Focus Areas

- Web application security
- API security
- Authentication and authorization
- Access control testing
- HTTP traffic analysis
- Vulnerability remediation

## Purpose

The goal of this repository is to document practical security research, lab findings, and remediation-focused write-ups.

All testing documented here is performed on intentionally vulnerable labs, systems I own, or systems where I have explicit authorization to test.
## Case Studies

- [Account Recovery Bypass](case-studies/F-003-account-recovery-bypass.md)
- [Cross-Tenant Authentication](case-studies/F-005-cross-tenant-authentication.md)
- [AVIF Security Validation](case-studies/F-002-avif-security-validation.md)

## Methodology

My security work focuses on controlled, evidence-driven validation.

Typical workflow:

1. Define scope and safety boundaries
2. Establish a negative control or baseline
3. Reproduce the suspected security behavior
4. Validate impact using authorized synthetic accounts or test data
5. Avoid destructive actions and unnecessary access to real user data
6. Document confirmed evidence separately from unverified assumptions
7. Restore modified test state where applicable
8. Provide remediation guidance

## Responsible Testing

Testing documented in this repository is performed only on systems I own, intentionally vulnerable environments, or systems where I have explicit authorization to assess.

Public case studies are sanitized to exclude credentials, tokens, real user information, exact exploit inputs, and other sensitive operational details.

## Research Focus

- Web application security
- API security
- Authentication and authorization
- Account recovery flows
- Multi-tenant security boundaries
- Session and token handling
- Vulnerability validation
- Remediation analysis
