# Playbook: Specification Conflict

## Trigger
Two authoritative sources, or the spec and repository evidence, conflict.

## Procedure
Determine the subject: business, API, DB, security, build, deployment. Apply authority by subject. If equal-authority sources conflict without supersession evidence, create a Decision Request.

```text
FACT A + source
FACT B + source
SUBJECT AUTHORITY
IMPACT
OPTIONS
```

## Forbidden Response
Do not apply a universal rule that “code is always right” or “spec is always right”; do not blend two conflicting semantics into a third solution.

## Exit Criteria
The decision has authority, downstream context is refreshed, and the affected candidate is reverified.
