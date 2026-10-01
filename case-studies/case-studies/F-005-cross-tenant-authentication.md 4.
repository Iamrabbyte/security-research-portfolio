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
