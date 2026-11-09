---
name: firearm-store-ledger-entries
description: "firearm-store guide for accounting-specialist: Turn transaction reports into balanced mock ledger entries."
user-invocable: false
---

# Ledger Entries

| | |
|---|---|
| **ID** | `ledger-entries` |
| **Used by** | [accounting-specialist](../../agents/firearm-store-accounting-specialist.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Turn transaction reports into balanced mock ledger entries.

## Approach

1. One entry per transaction report: sale → revenue and cash/receivable; purchase → inventory and cash/payable.
2. Debits must equal credits.
3. Reference the case ID and the transaction report.

## Watch Out

- Never create entries without a transaction report or another recorded financial event.

This skill is the source of truth for this guide.
