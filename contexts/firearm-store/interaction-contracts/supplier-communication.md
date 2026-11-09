# Supplier Communication

| | |
|---|---|
| **ID** | `supplier-communication` |
| **Requester** | [purchasing-orchestrator](../../../.claude/agents/firearm-store-purchasing-orchestrator.md) |
| **Provider** | [supplier](../../../.claude/agents/firearm-store-supplier.md) |

## Purpose

Purchasing asks a supplier for quotes, places orders, and requests returns.

## Input

- Request type: quote, order, or return.
- Items and quantities.
- Reason (for returns).

## Expected Output

Quote, order confirmation, or return decision (mock).

## Outcomes

- `quoted`
- `confirmed`
- `return-accepted`
- `return-refused`
- `cannot-determine`

## Rules

- External party: purchasing always validates the response before using it.
