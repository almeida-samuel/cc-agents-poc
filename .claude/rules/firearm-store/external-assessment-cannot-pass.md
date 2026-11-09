---
paths:
  - "data/firearm-store/**"
---

# External Assessment Cannot Replace Inspection

| | |
|---|---|
| **ID** | `external-assessment-cannot-pass` |
| **Scope** | Contract: [external-technical-assessment](../../../contexts/firearm-store/interaction-contracts/external-technical-assessment.md) |
| **Binds** | [technical-services-orchestrator](../../agents/firearm-store-technical-services-orchestrator.md), [external-technical-specialist](../../agents/firearm-store-external-technical-specialist.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

An external technical assessment follows an `inconclusive` physical inspection.

## Consequence

It can support `hold` or `reject`, but not `release`. Release needs a new physical inspection with a `pass` result.

## Check

A `release` disposition never references an external assessment as its pass evidence.
