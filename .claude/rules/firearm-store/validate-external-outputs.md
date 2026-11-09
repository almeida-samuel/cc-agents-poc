---
paths:
  - "data/firearm-store/**"
---

# Validate External Outputs

| | |
|---|---|
| **ID** | `validate-external-outputs` |
| **Scope** | Context (applies everywhere) |
| **Binds** | [purchasing-orchestrator](../../agents/firearm-store-purchasing-orchestrator.md), [technical-services-orchestrator](../../agents/firearm-store-technical-services-orchestrator.md), [compliance-orchestrator](../../agents/firearm-store-compliance-orchestrator.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

An external party returns an output.

## Consequence

The requesting orchestrator validates it against the expected output before using it. Invalid outputs are **rejected** and requested again (up to 3 retries), then escalated.

## Check

Every external response is followed by an `accepted` or `rejected` validation event from the requesting orchestrator.
