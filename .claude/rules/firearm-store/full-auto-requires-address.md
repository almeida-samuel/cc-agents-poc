---
paths:
  - "data/firearm-store/**"
---

# Full-Auto Requires Customer Address

| | |
|---|---|
| **ID** | `full-auto-requires-address` |
| **Scope** | Unit: [compliance](../../../contexts/firearm-store/units/compliance.md) |
| **Binds** | [documentation-specialist](../../agents/firearm-store-documentation-specialist.md), [compliance-orchestrator](../../agents/firearm-store-compliance-orchestrator.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

Any requested item has `full-auto` among its fire modes and the customer's address is missing from the case.

## Consequence

Documentation status is `needs-information`, listing `customer.address`. Sales returns it to the operator in the case response.

## Check

`'full-auto' in item.fire_modes and not case.customer.address` → status must be `needs-information`.
