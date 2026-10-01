# Cross-Tenant Authentication — Sanitized Case Study

## Summary

During an authorized black-box security assessment, I identified a potential cross-tenant authentication boundary issue affecting a multi-tenant web application.

Testing was performed exclusively with an authorized synthetic test account.

## Observation

A valid JWT issued after a normal login to one tenant was accepted by protected API endpoints associated with other distinct tenant identities.

As a negative control, an invalid bearer token was rejected with HTTP 401.

The same valid authentication token received HTTP 200 responses across multiple tenant contexts.

## Token Analysis

The observed JWT contained the following claim names:

- exp
- iat
- iss
- sub

No tenant ID, tenant-domain, or audience claim was observed that explicitly bound the token to the issuing tenant.

## Potential Impact

If the tenant identifiers represent separate security boundaries, acceptance of the same session token across tenants may indicate insufficient tenant isolation.

If the domains are intentionally configured as trusted mirrors sharing one identity boundary, this behavior may instead be expected.

For this reason, final severity depends on the intended tenant-isolation model.

## Validation Method

The behavior was validated using:

- an authorized synthetic account;
- a normally issued authentication token;
- an invalid-token negative control;
- comparison of protected API behavior across distinct tenant contexts;
- JWT claim inspection.

## Safety

No real user account was accessed.

No password or profile data was modified.

No payment, KYC, or financial data was accessed.

No brute force, denial-of-service, or destructive testing was performed.

## Disclosure Note

Target domains, raw JWT values, account identifiers, request identifiers, and exact endpoint details are intentionally omitted from this public portfolio.

## Root Cause Analysis

The observed behavior suggests that authenticated session state was not explicitly bound to the tenant context in which it was issued.

The validation showed that:

- an invalid bearer token was rejected;
- a valid JWT issued in one tenant context was accepted by protected endpoints in additional tenant contexts;
- the token did not contain an observed tenant ID, tenant-domain, or audience claim binding it to the issuing tenant.

This indicates a potential tenant-boundary enforcement weakness if those tenant identities are intended to operate as separate security domains.

## Classification

Potential classifications include:

**CWE-639 — Authorization Bypass Through User-Controlled Key**

and, depending on the intended trust model:

**CWE-862 — Missing Authorization**

Final classification depends on the application's intended tenant-isolation architecture.

## Remediation

Recommended remediation includes:

- explicitly bind authenticated sessions or tokens to the intended tenant context;
- validate tenant identity server-side on every protected request;
- enforce tenant authorization independently of client-controlled routing or host context;
- use an appropriate audience or tenant-specific claim where the architecture supports it;
- reject tokens presented outside their authorized tenant boundary;
- verify that shared identity infrastructure does not unintentionally grant cross-tenant resource access;
- re-test protected endpoints after remediation.

## Validation Timeline

- **2026-09-30** — Initial cross-tenant validation performed using an authorized synthetic account.
- **2026-09-30** — Invalid-token negative control returned HTTP 401.
- **2026-09-30** — The same valid JWT was accepted across multiple distinct tenant identities.
- **2026-09-30** — JWT claims were reviewed for explicit tenant or audience binding.
- **2026-09-30** — Final severity was left conditional on confirmation of the intended tenant-isolation model.

## Evidence Quality

The finding was supported by:

- an invalid-token negative control;
- protected endpoint responses;
- comparison across multiple distinct tenant identities;
- inspection of JWT claim names;
- use of a single authorized synthetic test account;
- preservation of sensitive authentication material outside the public portfolio.

No real customer account, payment data, KYC data, or private document content was accessed during validation.

## Severity Note

This finding should not be treated as definitively high severity unless the affected tenant identities are intended to represent separate security boundaries.

If the environments are intentionally configured as trusted mirrors sharing one identity domain, the observed behavior may be expected.
