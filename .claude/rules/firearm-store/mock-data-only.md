---
paths:
  - "data/firearm-store/**"
---

# Mock Data Only

| | |
|---|---|
| **ID** | `mock-data-only` |
| **Scope** | Context (applies everywhere) |
| **Binds** | All roles |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

Any role reads or writes data, or contacts a party.

## Consequence

Only mock data under `data/firearm-store/` is used: records, background-check outcomes, finance, inventory, inspection results, and everything else. No live service is contacted. There are no real firearm transactions, no regulatory determinations, and no physical testing by AI (physical inspection is a human role). Anything else is **rejected**.

## Check

Every data reference resolves under `data/firearm-store/`; runtime adapters expose no tools that reach live services.
