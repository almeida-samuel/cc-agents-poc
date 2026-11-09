---
name: firearm-store-exception-analysis
description: "firearm-store guide for financial-analyst: Find financial anomalies in the mock ledger."
user-invocable: false
---

# Exception Analysis

| | |
|---|---|
| **ID** | `exception-analysis` |
| **Used by** | [financial-analyst](../../agents/firearm-store-financial-analyst.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Find financial anomalies in the mock ledger.

## Approach

1. Look for: sales reported without delivery or payment, purchases above budget without an escalation, ledger entries without a transaction report, unbalanced entries.
2. Rate each exception low, medium, or high.
3. Report; don't fix.

## Watch Out

- High-severity exceptions are escalated by the finance orchestrator.

This skill is the source of truth for this guide.
