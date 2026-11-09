---
paths:
  - "data/firearm-store/**"
---

# Sale Completion

| | |
|---|---|
| **ID** | `sale-completion` |
| **Scope** | Context (applies everywhere) |
| **Binds** | [sales-orchestrator](../../agents/firearm-store-sales-orchestrator.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

Sales decides whether a sale is completed.

## Consequence

A sale is completed only when payment is `paid` **and** the order is `delivered`. Only then is it reported.

## Check

`payment.status == 'paid' and delivery.status == 'delivered'` before any sale `transaction-report` event.
