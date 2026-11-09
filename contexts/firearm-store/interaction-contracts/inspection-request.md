# Inspection Request

| | |
|---|---|
| **ID** | `inspection-request` |
| **Requester** | [purchasing-orchestrator](../../../.claude/agents/firearm-store-purchasing-orchestrator.md) or [inventory-orchestrator](../../../.claude/agents/firearm-store-inventory-orchestrator.md) |
| **Provider** | [technical-services-orchestrator](../../../.claude/agents/firearm-store-technical-services-orchestrator.md) |

## Purpose

Purchasing (used items) or inventory (incoming new stock) asks technical services to inspect an item before it can be accepted.

## Input

- Case ID.
- Item ID.
- New or used.
- Documents and maintenance history, if any.

## Expected Output

Inspection result and the disposition technical services will send to inventory.

## Outcomes

- `pass`
- `needs-work`
- `reject`
- `inconclusive`
- `cannot-determine`

## Rules

- `inconclusive` never leads to release.
