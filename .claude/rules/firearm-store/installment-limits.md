---
paths:
  - "data/firearm-store/**"
---

# Installment Limits

| | |
|---|---|
| **ID** | `installment-limits` |
| **Scope** | Unit: [finance](../../../contexts/firearm-store/units/finance.md) |
| **Binds** | [finance-orchestrator](../../agents/firearm-store-finance-orchestrator.md), [payments-credit-specialist](../../agents/firearm-store-payments-credit-specialist.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

An installment plan is requested.

## Consequence

Plans are offered only if the customer's mock credit score is **≥ 650**. Financed amounts above **$5,000** per customer are **escalated** to the general manager. Only the plans in `data/firearm-store/records/finance/installment-terms.json` may be offered.

## Check

`credit_score >= 650`; `financed_amount <= 5000` or an escalation event exists; plan terms ∈ `data/firearm-store/records/finance/installment-terms.json`.
