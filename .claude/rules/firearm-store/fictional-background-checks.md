---
paths:
  - "data/firearm-store/**"
---

# Background Checks Are Fictional

| | |
|---|---|
| **ID** | `fictional-background-checks` |
| **Scope** | Context (applies everywhere) |
| **Binds** | [government-background-check](../../agents/firearm-store-government-background-check.md), [regulatory-verification-specialist](../../agents/firearm-store-regulatory-verification-specialist.md), [compliance-orchestrator](../../agents/firearm-store-compliance-orchestrator.md), [sales-orchestrator](../../agents/firearm-store-sales-orchestrator.md), [customer-support](../../agents/firearm-store-customer-support.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

A background-check result is produced, recorded, or reported.

## Consequence

It is labeled fictional and never presented as a real legal eligibility determination. An unlabeled result is **rejected**.

## Check

`fictional == true` on every background-check result and interpretation; case responses quote the outcome code, not an eligibility statement.
