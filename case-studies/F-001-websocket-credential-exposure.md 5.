# WebSocket Credential Exposure — Historical Validation

## Summary

During an authorized black-box security assessment, a credential exposed in public frontend content was initially observed to authenticate successfully to a WebSocket service.

The finding was later re-tested and was no longer reproducible at the current route state.

## Initial Validation

The public frontend exposed a WebSocket-related credential.

During the original validation:

- the exposed credential resulted in a successful WebSocket upgrade;
- requests without the credential were rejected;
- requests using an invalid credential were rejected.

This established a meaningful authentication difference at the time of testing.

## Revalidation

During a later revalidation attempt, the upstream route no longer returned a WebSocket upgrade for any tested case.

Instead, the tested requests received the same ordinary HTTP response.

Because the previous behavior could not be reproduced, the authentication value of the exposed credential was not represented as currently confirmed.

## Evidence Handling

The public version of this case study intentionally excludes:

- the credential value;
- target domains;
- exact connection details;
- private channel identifiers;
- raw authentication material.

## Current Classification

- Public credential exposure: historically observed
- Successful WebSocket authentication: historically validated
- Current authentication value: not reconfirmed
- Private-feed access: not proven
- Mutation or publish capability: not proven

## Security Practice Demonstrated

This case study demonstrates the importance of:

- preserving historical evidence;
- performing revalidation;
- distinguishing historical observations from current reproducibility;
- avoiding escalation of severity when impact is not currently confirmed.

## Responsible Testing

No customer account, user identifier, financial action, subscription payload, or mutation request was targeted during the validation.
