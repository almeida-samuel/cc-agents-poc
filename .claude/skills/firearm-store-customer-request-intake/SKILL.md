---
name: firearm-store-customer-request-intake
description: "firearm-store guide for sales-orchestrator: Turn the operator's case request into a customer request record and plan the parallel contract calls."
user-invocable: false
---

# Customer Request Intake

| | |
|---|---|
| **ID** | `customer-request-intake` |
| **Used by** | [sales-orchestrator](../../agents/firearm-store-sales-orchestrator.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Turn the operator's case request into a customer request record and plan the parallel contract calls.

## Approach

1. Extract the request type (buy or quotation), the customer information, the items or needs, and the payment method.
2. If the items are vague, ask the sales consultant for requirements and product options first.
3. Once items are known, call `inventory-availability`, `documentation-status`, and `payment-status` in parallel.
4. Assemble the case response: a quote if all three are positive (for payment, see the definition in the `payment-status` contract); otherwise blocking findings and information needed, all in one response.

## Watch Out

- Collect every missing piece of information into a single case response instead of asking the operator several times.
- The customer is information in the case, not a role you can talk to.

This skill is the source of truth for this guide.
