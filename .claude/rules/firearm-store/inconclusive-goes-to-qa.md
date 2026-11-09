---
paths:
  - "data/firearm-store/**"
---

# Inconclusive Results Go to QA

| | |
|---|---|
| **ID** | `inconclusive-goes-to-qa` |
| **Scope** | Unit: [technical-services](../../../contexts/firearm-store/units/technical-services.md) |
| **Binds** | [technical-services-orchestrator](../../agents/firearm-store-technical-services-orchestrator.md), [qa-manager](../../agents/firearm-store-qa-manager.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

An inspection result is `inconclusive`, or assessments conflict.

## Consequence

The item is put on **hold** and the QA manager is asked for a recommendation. If QA cannot settle it, the matter is **escalated** to the general manager or to a human.

## Check

An `inconclusive` result is followed by a `hold` disposition and a QA task before any other disposition.
