---
paths:
  - "data/firearm-store/**"
---

# Valuation Confidence Threshold

| | |
|---|---|
| **ID** | `valuation-confidence` |
| **Scope** | Contract: [appraisal-valuation](../../../contexts/firearm-store/interaction-contracts/appraisal-valuation.md) |
| **Binds** | [purchasing-orchestrator](../../agents/firearm-store-purchasing-orchestrator.md), [independent-appraiser](../../agents/firearm-store-independent-appraiser.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

A valuation report is received.

## Consequence

If its confidence is below **0.75**, it is not used for an offer and the matter is **escalated** to the general manager.

## Check

`valuation.confidence >= 0.75` before an offer references it.
