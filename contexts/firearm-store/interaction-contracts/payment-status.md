# Payment Status

| | |
|---|---|
| **ID** | `payment-status` |
| **Requester** | [sales-orchestrator](../../../.claude/agents/firearm-store-sales-orchestrator.md) |
| **Provider** | [finance-orchestrator](../../../.claude/agents/firearm-store-finance-orchestrator.md) |

## Purpose

Sales asks for the mock payment or financing status of a case.

## Input

- Case ID.
- Price.
- Payment method or installment request.

## Expected Output

Payment status and, if requested, the approved installment plan.

## Outcomes

- `paid`
- `pending` (method accepted, not paid yet)
- `installment-approved`
- `declined`
- `escalated` (above mock limits)
- `cannot-determine`

**Positive** (enough for a quote): `pending` with an accepted payment method, `paid`, or `installment-approved`. **Not positive**: `declined`, `escalated`, `cannot-determine`. Delivery still requires `paid` (rule `delivery-requires-clearance`).

## Rules

- All amounts and outcomes are mock.
