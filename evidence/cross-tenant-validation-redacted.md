# Cross-Tenant Authentication Validation — Redacted Evidence

## Context

This document is a sanitized validation excerpt from an authorized black-box security assessment.

The test used a single authorized synthetic account to determine whether an authenticated session issued in one tenant context was accepted by protected resources associated with other tenant contexts.

Target identities, raw tokens, exact endpoint paths, and reusable operational details have been intentionally removed.

## Redaction Notice

The public version does not include:

- target domains or URLs
- tenant identifiers
- usernames or user IDs
- exact endpoint paths
- request identifiers
- raw JWT values
- credentials
- private data
- reusable authentication material

## Negative Control

Before testing cross-tenant session behavior, an invalid bearer token was submitted to a protected authenticated resource.

Observed result:

- HTTP status: `401`
- authentication rejected

This established a negative control showing that arbitrary bearer values were not accepted.

## Source-Tenant Authentication

The authorized synthetic account was authenticated normally in the original tenant context.

A valid JWT session was issued through the ordinary authentication flow.

The raw JWT is intentionally not published.

## Cross-Tenant Validation

The same valid authenticated session was then presented to protected resources associated with additional tenant contexts.

Observed behavior:

### Tenant Context A

- valid session accepted
- protected authenticated response: HTTP `200`

### Tenant Context B

- same session accepted
- protected authenticated response: HTTP `200`

### Tenant Context C

- same session accepted
- protected authenticated response: HTTP `200`

The same authenticated session was therefore accepted across multiple distinct tenant identities.

## JWT Structure Review

The raw JWT was not stored in this public evidence.

The decoded claim names included:

- `exp`
- `iat`
- `iss`
- `sub`

No explicit claim was observed that bound the token to:

- a tenant identifier
- a tenant domain
- an audience

This observation does not independently prove an authorization vulnerability, but it is consistent with the cross-tenant session behavior observed during validation.

## Impact Boundary

The validation demonstrated session acceptance across different tenant contexts.

The assessment did not demonstrate:

- access to another real customer's account
- access to another user's private documents
- financial actions
- KYC data access
- payment data access
- password modification
- privilege escalation
- destructive changes

The synthetic account's protected resources were used only to establish whether the same session was accepted across tenant contexts.

## Interpretation

If the tested tenant identities are intended to operate as separate authorization boundaries, the observed behavior is consistent with insufficient tenant isolation.

If the tenant identities are intentionally configured as trusted mirrors sharing one authentication and authorization domain, the behavior may be expected.

For this reason, final impact remains dependent on the intended tenant-isolation model.

## Evidence Calibration

The available evidence supports the following observations:

- an invalid bearer token was rejected;
- a valid JWT was issued through normal authentication;
- the same JWT was accepted across multiple distinct tenant contexts;
- the JWT did not expose an explicit tenant or audience binding claim.

The evidence does not independently establish the application's intended trust relationship between those tenant contexts.

## Safety

- Only an authorized synthetic account was used.
- No real customer account was accessed.
- No raw JWT is published.
- No password was modified.
- No profile data was changed.
- No financial action was performed.
- No private document content was extracted.
- No brute force was used.
- No destructive testing was performed.
- No denial-of-service activity was performed.

## Conclusion

The validation demonstrated that a normally issued authenticated session was accepted across multiple distinct tenant contexts while an invalid bearer token was rejected.

The security significance of that behavior depends on whether those tenant contexts are intended to represent separate authorization boundaries.
