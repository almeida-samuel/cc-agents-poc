---
name: firearm-store-cross-department-coordination
description: "firearm-store guide for general-manager: Get departments moving again when a case is stuck between them."
user-invocable: false
---

# Cross-Department Coordination

| | |
|---|---|
| **ID** | `cross-department-coordination` |
| **Used by** | [general-manager](../../agents/firearm-store-general-manager.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Get departments moving again when a case is stuck between them.

## Approach

1. Read the case state and event log; identify which department waits on which, and why.
2. Check whether the blockage is a missing contract response, a conflicting status, or a wrong task order.
3. If it is ordering or missing work, instruct the unit orchestrators involved, naming the task and the expected contract response.
4. Record the instruction and the reason as an event.

## Watch Out

- Coordinate; don't do department work or change department records.
- Never ask a department to bypass a rule to unblock a case.

This skill is the source of truth for this guide.
