# Cross-Tenant Authentication Boundary Validation

## Summary

During an authorized black-box assessment, I investigated whether an authenticated session issued in one tenant context was accepted by protected API resources associated with other tenant contexts.

Testing was performed exclusively with an authorized synthetic test account.

The assessment confirmed that the same valid JWT was accepted across multiple distinct tenant identities, while an invalid bearer token was rejected.

Final security impact depends on whether those tenant identities are intended to represent separate authorization boundaries.

## Confirmed Behavior

A valid JWT was obtained through a normal login flow in one tenant context.

Observed behavior:

- invalid bearer token: HTTP 401
- valid JWT accepted in the issuing tenant
- same valid JWT accepted by protected endpoints in additional tenant contexts
- successful authenticated responses observed across multiple distinct tenant identities

This demonstrated that authenticated session state was accepted outside the tenant context in which it had originally been issued.

## Token Analysis

The observed JWT contained the following claim names:

- `exp`
- `iat`
- `iss`
- `sub`

No explicit tenant ID, tenant-domain, or audience claim was observed that bound the token to the issuing tenant context.

Raw JWT material is intentionally excluded from this public case study.

## Root Cause Analysis

The observed behavior suggests that authorization enforcement did not explicitly bind the authenticated session to the tenant context in which it was issued.

Authentication itself succeeded normally.

The security concern appears at the authorization boundary: a session originating from one tenant context was accepted when presented to protected resources associated with other tenant contexts.

If those tenants are intended to operate as separate security domains, this indicates insufficient server-side enforcement of tenant isolation.

If the environments are intentionally configured as trusted mirrors sharing one identity and authorization domain, the observed behavior may be expected.

## Classification

**Primary classification: CWE-863 — Incorrect Authorization**

The demonstrated issue concerns authorization across tenant boundaries.

A valid session was accepted by resources associated with additional tenant contexts, suggesting that tenant-specific authorization checks may not have been enforced.

Final classification depends on the application's intended trust and isolation model.

## Impact

If the tested tenant identities are intended to represent separate security boundaries, the observed behavior could allow an authenticated user from one tenant context to access protected functionality associated with another.

Potential consequences may include:

- cross-tenant access;
- bypass of intended tenant isolation;
- access beyond the originally authenticated security domain;
- exposure of tenant-specific protected resources.

No access to another real user's private data was demonstrated during this assessment.

## CVSS

No unconditional production CVSS score is assigned because the tenant-isolation model was not independently confirmed.

### Illustrative isolated-tenant scenario

If the tested tenants are intended to be separate authorization boundaries and successful reuse grants access to protected tenant resources, an illustrative CVSS v3.1 vector would be:

`CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N`

**Illustrative base score: 6.5 (Medium)**

This scenario assumes:

- network-reachable exploitation;
- a valid low-privilege account is required;
- no user interaction is required;
- unauthorized cross-tenant confidentiality impact is significant;
- no demonstrated integrity or availability impact.

This is not a vendor-assigned score and should not be applied if the tenants intentionally share one authorization boundary.

## Remediation

Recommended actions include:

- explicitly bind authenticated sessions or tokens to the intended tenant context;
- validate tenant identity server-side on every protected request;
- enforce tenant authorization independently of client-controlled routing or host context;
- use tenant-specific or audience-specific token claims where appropriate;
- verify those claims on every protected request;
- reject sessions presented outside their authorized tenant boundary;
- review shared authentication infrastructure for unintended cross-tenant trust;
- revalidate protected endpoints after remediation.

## Validation Timeline

- **2026-09-30** — Authorized synthetic test account authenticated normally.
- **2026-09-30** — Invalid-token negative control returned HTTP 401.
- **2026-09-30** — Valid JWT accepted in the issuing tenant context.
- **2026-09-30** — Same JWT accepted by protected endpoints in additional tenant contexts.
- **2026-09-30** — JWT claims reviewed for tenant or audience binding.
- **2026-09-30** — Final severity left conditional on confirmation of the intended tenant-isolation model.

## Disclosure Status

The timeline above describes technical validation activity only.

No public claim is made here regarding:

- vendor acknowledgement;
- bounty-program acceptance;
- coordinated disclosure;
- remediation attribution.

## Evidence Handling

The public version intentionally excludes:

- target domains and URLs;
- raw JWT values;
- account identifiers;
- request identifiers;
- exact endpoint paths;
- credentials;
- private user data;
- reusable operational details.

The purpose of this case study is to document the authorization behavior and validation methodology without exposing target-specific secrets or attack material.

## Safety Boundary

Testing was limited to a single authorized synthetic account.

No real customer account was accessed.

No password or profile data was modified.

No payment, KYC, private-document, or financial data was accessed.

No brute force, denial-of-service, destructive payload, or persistence technique was used.
