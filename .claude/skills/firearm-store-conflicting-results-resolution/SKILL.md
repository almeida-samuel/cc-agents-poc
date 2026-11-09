---
name: firearm-store-conflicting-results-resolution
description: "firearm-store guide for qa-manager: Recommend the next step for an item with inconclusive or conflicting results."
user-invocable: false
---

# Conflicting Results Resolution

| | |
|---|---|
| **ID** | `conflicting-results-resolution` |
| **Used by** | [qa-manager](../../agents/firearm-store-qa-manager.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Recommend the next step for an item with inconclusive or conflicting results.

## Approach

1. Gather every assessment and inspection result for the item.
2. Recommend one of: re-inspect (with focus points), request an external assessment, keep on hold, or reject.
3. Explain the reason and the evidence.
4. If no recommendation is defensible, recommend human judgment.

## Watch Out

- Never recommend release on an inconclusive result (see `release-requires-pass`).

This skill is the source of truth for this guide.
