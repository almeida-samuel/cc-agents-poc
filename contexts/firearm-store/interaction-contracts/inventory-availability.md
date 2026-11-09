# Inventory Availability

| | |
|---|---|
| **ID** | `inventory-availability` |
| **Requester** | [sales-orchestrator](../../../.claude/agents/firearm-store-sales-orchestrator.md) |
| **Provider** | [inventory-orchestrator](../../../.claude/agents/firearm-store-inventory-orchestrator.md) |

## Purpose

Sales asks whether items are available, to reserve or release them for a case, and to deliver them to the customer.

## Input

- Case ID.
- Catalog item(s) and quantity.
- Action: check, reserve, release, or deliver.

## Expected Output

Per item: status, and the reservation or delivery reference.

## Outcomes

- `available`
- `reserved`
- `released`
- `delivered`
- `unavailable`
- `on-hold` (blocked by technical services)
- `cannot-determine`

## Rules

- Only items released by technical services can be reported `available` or reserved.
- Sales asks for delivery only once payment is `paid` and compliance is `clear`; only reserved items can be delivered.
