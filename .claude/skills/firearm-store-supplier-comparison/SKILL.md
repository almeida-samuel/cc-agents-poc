---
name: firearm-store-supplier-comparison
description: "firearm-store guide for procurement-analyst: Recommend the best supplier option."
user-invocable: false
---

# Supplier Comparison

| | |
|---|---|
| **ID** | `supplier-comparison` |
| **Used by** | [procurement-analyst](../../agents/firearm-store-procurement-analyst.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Recommend the best supplier option.

## Approach

1. Compare total cost, lead time, and past prices from the mock price history.
2. Recommend one option with the reason; note if all options exceed the $20,000 budget (see `purchasing-budget`).

## Watch Out

- Use only quotes received in the case and the mock price history.

This skill is the source of truth for this guide.
