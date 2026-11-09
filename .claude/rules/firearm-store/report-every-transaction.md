---
paths:
  - "data/firearm-store/**"
---

# Report Every Transaction

| | |
|---|---|
| **ID** | `report-every-transaction` |
| **Scope** | Contract: [transaction-report](../../../contexts/firearm-store/interaction-contracts/transaction-report.md) |
| **Binds** | [sales-orchestrator](../../agents/firearm-store-sales-orchestrator.md), [purchasing-orchestrator](../../agents/firearm-store-purchasing-orchestrator.md), [finance-orchestrator](../../agents/firearm-store-finance-orchestrator.md), [inventory-orchestrator](../../agents/firearm-store-inventory-orchestrator.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

A sale is completed or a purchase is made.

## Consequence

It is reported to **both** finance and inventory. It counts as reported only when both have recorded it; until then the case cannot close.

## Check

Every completed sale and every purchase has `recorded` events from both `finance-orchestrator` and `inventory-orchestrator`.
