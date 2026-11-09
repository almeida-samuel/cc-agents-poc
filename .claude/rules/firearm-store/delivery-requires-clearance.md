---
paths:
  - "data/firearm-store/**"
---

# Delivery Requires Clearance

| | |
|---|---|
| **ID** | `delivery-requires-clearance` |
| **Scope** | Contract: [inventory-availability](../../../contexts/firearm-store/interaction-contracts/inventory-availability.md) |
| **Binds** | [sales-orchestrator](../../agents/firearm-store-sales-orchestrator.md), [inventory-orchestrator](../../agents/firearm-store-inventory-orchestrator.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

Sales asks inventory to deliver an item.

## Consequence

Allowed only when the item is reserved for the case, compliance is `clear`, and payment is `paid`. Otherwise inventory **refuses** the delivery.

## Check

`reservation.case_id == case.id and compliance.flag == 'clear' and payment.status == 'paid'`.
