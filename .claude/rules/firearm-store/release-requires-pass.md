---
paths:
  - "data/firearm-store/**"
---

# Release Requires a Pass Result

| | |
|---|---|
| **ID** | `release-requires-pass` |
| **Scope** | Context (applies everywhere) |
| **Binds** | [technical-services-orchestrator](../../agents/firearm-store-technical-services-orchestrator.md), [inventory-orchestrator](../../agents/firearm-store-inventory-orchestrator.md), [qa-manager](../../agents/firearm-store-qa-manager.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

An item (new or used) would become sellable.

## Consequence

Only with a `release` disposition backed by a `pass` inspection result. `inconclusive`, `needs-work`, and `reject` never lead to release; inventory **refuses** a release without a pass.

## Check

`disposition == 'release'` → referenced `inspection_result.outcome == 'pass'`.
