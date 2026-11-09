---
paths:
  - "data/firearm-store/**"
---

# Purchasing Budget

| | |
|---|---|
| **ID** | `purchasing-budget` |
| **Scope** | Unit: [purchasing](../../../contexts/firearm-store/units/purchasing.md) |
| **Binds** | [purchasing-orchestrator](../../agents/firearm-store-purchasing-orchestrator.md), [procurement-analyst](../../agents/firearm-store-procurement-analyst.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

A supplier quote is about to be accepted.

## Consequence

Quotes up to **$20,000** per order may be accepted; above that the decision is **escalated** to the general manager.

## Check

`quote.total <= 20000` or an escalation event exists.
