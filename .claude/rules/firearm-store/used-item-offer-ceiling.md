---
paths:
  - "data/firearm-store/**"
---

# Used-Item Offer Ceiling

| | |
|---|---|
| **ID** | `used-item-offer-ceiling` |
| **Scope** | Unit: [purchasing](../../../contexts/firearm-store/units/purchasing.md) |
| **Binds** | [purchasing-orchestrator](../../agents/firearm-store-purchasing-orchestrator.md), [used-item-acquisition-specialist](../../agents/firearm-store-used-item-acquisition-specialist.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

A used-item offer is prepared or made.

## Consequence

The offer may not exceed **70%** of the midpoint of the accepted valuation range, and requires a `pass` inspection result. Higher offers are **escalated**.

## Check

`offer.amount <= 0.7 * valuation.midpoint` and `inspection_result.outcome == 'pass'`.
