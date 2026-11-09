---
paths:
  - "data/firearm-store/**"
---

# Reservation Expiry

| | |
|---|---|
| **ID** | `reservation-expiry` |
| **Scope** | Unit: [inventory](../../../contexts/firearm-store/units/inventory.md) |
| **Binds** | [inventory-orchestrator](../../agents/firearm-store-inventory-orchestrator.md), [stock-manager](../../agents/firearm-store-stock-manager.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

A reservation is older than **72 hours** and the order is not delivered.

## Consequence

The reservation is **released** and sales is told.

## Check

`now - reservation.created_at > 72h and not delivered` → release.
