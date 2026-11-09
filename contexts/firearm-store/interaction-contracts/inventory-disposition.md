# Inventory Disposition

| | |
|---|---|
| **ID** | `inventory-disposition` |
| **Requester** | [technical-services-orchestrator](../../../.claude/agents/firearm-store-technical-services-orchestrator.md) |
| **Provider** | [inventory-orchestrator](../../../.claude/agents/firearm-store-inventory-orchestrator.md) |

## Purpose

Technical services tells inventory whether an inspected item is released, held, or rejected.

## Input

- Item ID.
- Disposition: release, hold, or reject.
- Reference to the inspection result.

## Expected Output

Confirmation that inventory applied the disposition.

## Outcomes

- `applied`
- `refused` (e.g., release without a pass result)

## Rules

- Release requires a pass result.
- `inconclusive`, `needs-work`, and `reject` map to hold or reject.
