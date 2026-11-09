---
paths:
  - "data/firearm-store/**"
---

# Received Items Unavailable Until Released

| | |
|---|---|
| **ID** | `received-items-unavailable` |
| **Scope** | Unit: [inventory](../../../contexts/firearm-store/units/inventory.md) |
| **Binds** | [inventory-orchestrator](../../agents/firearm-store-inventory-orchestrator.md), [receiving-operations](../../agents/firearm-store-receiving-operations.md), [stock-manager](../../agents/firearm-store-stock-manager.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

An item is received (new stock or used acquisition).

## Consequence

Its status stays `on-hold` until technical services sends a `release` disposition.

## Check

New item records start `on-hold`; status `available` requires a `release` disposition event.
