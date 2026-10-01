# Security Research Portfolio

Hands-on web application and API security research focused on controlled, evidence-driven validation.

All published work is based on intentionally vulnerable environments, systems I own, or systems where I have explicit authorization to test.

## Selected Case Studies

- [WebSocket Credential Exposure](case-studies/F-001-websocket-credential-exposure.md)
- [AVIF / Next.js Security Validation](case-studies/F-002-avif-security-validation.md)
- [Account Recovery Bypass](case-studies/F-003-account-recovery-bypass.md)
- [Cross-Tenant Authentication](case-studies/F-005-cross-tenant-authentication.md)

## Sample Report

- [Sanitized Penetration Test Report](sample-pentest-report.md)

## Research Focus

- Web application penetration testing
- API security
- Authentication and authorization
- Access control
- Account recovery and identity flows
- JWT and session security
- Multi-tenant isolation
- WebSocket security
- Vulnerability validation
- Remediation and revalidation

## Methodology

My workflow is built around reproducible evidence rather than scanner-only findings.

Typical process:

1. Define scope and safety boundaries
2. Establish a baseline or negative control
3. Map the relevant application or API behavior
4. Reproduce the suspected security issue
5. Validate impact using authorized synthetic accounts or test data
6. Separate confirmed behavior from theoretical exploitability
7. Restore modified test state where applicable
8. Document remediation guidance
9. Revalidate after remediation when possible

## Evidence Handling

Public case studies are intentionally sanitized.

The following are excluded from public reports where applicable:

- target domains and URLs
- credentials
- session tokens
- cookies
- verification values
- real user information
- exact exploit inputs
- sensitive endpoint details

This allows findings and methodology to be reviewed without exposing operational target information or reusable attack material.

## Responsible Testing

No real-user access, financial actions, destructive testing, denial-of-service activity, or unnecessary data access is included in the public portfolio.

Where a higher-impact condition could not be safely demonstrated, it is documented as unconfirmed rather than presented as proven.
