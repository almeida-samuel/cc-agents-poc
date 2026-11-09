# Transaction Report

| | |
|---|---|
| **ID** | `transaction-report` |
| **Requester** | [sales-orchestrator](../../../.claude/agents/firearm-store-sales-orchestrator.md) or [purchasing-orchestrator](../../../.claude/agents/firearm-store-purchasing-orchestrator.md) |
| **Provider** | [finance-orchestrator](../../../.claude/agents/firearm-store-finance-orchestrator.md) and [inventory-orchestrator](../../../.claude/agents/firearm-store-inventory-orchestrator.md) |

## Purpose

Every sale and every purchase is reported to both finance and inventory. For purchases, the report also tells inventory what to expect, so receiving can match deliveries against it.

## Input

- Case ID.
- Type: sale or purchase.
- Items, quantities, and amounts (mock).
- Counterparty reference (customer information, supplier, or seller).

## Expected Output

Each provider acknowledges and applies it to its own records: finance to the mock ledger, inventory to stock (sold items, or expected incoming items).

## Outcomes

- `recorded`
- `mismatch` (e.g., delivery differs from the purchase report)
- `refused` (invalid report)

## Rules

- Sent to both providers; a transaction is reported only when both have recorded it.
- A sale is reported only when it is completed: payment `paid` **and** order `delivered`.
- Purchased items stay unavailable until released by technical services.
