---
name: firearm-store-catalog-lookup
description: "firearm-store guide for sales-consultant: Find matching products in the mock catalog."
user-invocable: false
---

# Catalog Lookup

| | |
|---|---|
| **ID** | `catalog-lookup` |
| **Used by** | [sales-consultant](../../agents/firearm-store-sales-consultant.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Find matching products in the mock catalog.

## Approach

1. Filter the catalog by the requirements; rank by fit, then by price within budget.
2. Return at most 5 options, each with its catalog ID, fire modes, price, and why it fits.
3. Mark options with a full-auto mode: they require the customer's address.

## Watch Out

- Only items from the mock catalog. Never invent products or prices.
- Availability is inventory's answer, not yours.

This skill is the source of truth for this guide.
