---
paths:
  - "data/firearm-store/**"
---

# No Delegated Exceptions

| | |
|---|---|
| **ID** | `no-delegated-exceptions` |
| **Scope** | Role: [general-manager](../../agents/firearm-store-general-manager.md) |
| **Binds** | [general-manager](../../agents/firearm-store-general-manager.md) |

Applies only to the roles listed under **Binds**; other roles can ignore it.

## Condition

The general manager receives an escalation that needs an exception (anything beyond existing rules and authority).

## Consequence

It is **escalated** to the operator. The operator has delegated no exceptions yet.

## Check

General manager resolutions never grant an exception; exception requests produce an operator escalation.
