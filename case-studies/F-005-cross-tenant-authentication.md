# Cross-Tenant Authentication Boundary Case Study

## Summary

During an authorized black-box security assessment, I identified a potential cross-tenant authentication boundary issue affecting a multi-tenant web application.

Testing was performed exclusively with an authorized synthetic test account.

No real user data, financial transactions, denial-of-service testing, persistence, or unauthorized account access was involved.

## Finding

A valid JWT issued after normal authentication to one public tenant was accepted by protected API endpoints associated with multiple distinct public tenant identifiers.

As a negative control, an invalid token returned an HTTP 401 response.

The valid JWT received successful responses from the tested protected endpoints across the tested tenant contexts.

## Token Analysis

The observed JWT contained the following standard claims:

- `exp`
- `iat`
- `iss`
- `sub`

During testing, I did not observe a claim explicitly binding the token to a tenant, domain, or audience.

## Potential Impact

If the tenant identifiers represent intended security boundaries, acceptance of the same authentication token across tenants could indicate insufficient tenant isolation.

However, if the tested domains intentionally share a common identity boundary, the behavior may be expected.

For this reason, the finding requires confirmation of the application's intended tenant-isolation model before a final severity determination can be made.

## Validation Method

The behavior was validated using:

- an authorized synthetic account;
- a normally issued authentication token;
- an intentionally invalid token as a negative control;
- comparison of authentication behavior across distinct public tenant contexts;
- protected API responses;
- JWT claim inspection.

## Safety Controls

The assessment did not involve:

- real user accounts;
- brute force;
- denial-of-service activity;
- financial transactions;
- persistence;
- administrator access;
- modification of unrelated user data.

## Remediation Considerations

If tenants are intended to function as separate security boundaries, authentication and authorization controls should explicitly validate the tenant context associated with each request.

Possible controls include binding authentication state to an intended tenant or audience and enforcing that boundary server-side on protected resources.

## Disclosure

Target-identifying information, authentication material, request identifiers, account information, and exact endpoint details have intentionally been withheld from this public version.

Full technical evidence can be provided privately where appropriate and authorized.
