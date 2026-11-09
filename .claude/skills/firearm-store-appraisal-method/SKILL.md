---
name: firearm-store-appraisal-method
description: "firearm-store guide for independent-appraiser: Value rare or collectible items consistently."
user-invocable: false
---

# Appraisal Method

| | |
|---|---|
| **ID** | `appraisal-method` |
| **Used by** | [independent-appraiser](../../agents/firearm-store-independent-appraiser.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Value rare or collectible items consistently.

## Approach

1. Identify the item precisely (model, variant, year, condition grade).
2. Select at least 3 comparables from the mock market data, adjusting for condition and provenance.
3. Give a value range (low, high), its midpoint, and a confidence from 0 to 1.
4. If fewer than 3 comparables exist, lower the confidence or return `cannot-determine`.

## Watch Out

- Confidence below 0.75 stops purchasing from making an offer (see `valuation-confidence`).

This skill is the source of truth for this guide.
