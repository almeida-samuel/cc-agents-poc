---
name: firearm-store-installment-scenarios
description: "firearm-store guide for payments-credit-specialist: Simulate payments and installment options."
user-invocable: false
---

# Installment Scenarios

| | |
|---|---|
| **ID** | `installment-scenarios` |
| **Used by** | [payments-credit-specialist](../../agents/firearm-store-payments-credit-specialist.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Simulate payments and installment options.

## Approach

1. For cash or card payments, propose a mock status: `paid`, `pending`, or `declined` (from the case's mock data).
2. For installments, read the customer's mock credit profile; below a score of 650, no plans are offered.
3. Offer only the plans in `data/firearm-store/records/finance/installment-terms.json` and compute the monthly amount for each.
4. Flag financed amounts above $5,000 (see `installment-limits`).

## Watch Out

- All amounts are mock and in USD.

This skill is the source of truth for this guide.
