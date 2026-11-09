---
name: firearm-store-intake-recording
description: "firearm-store guide for receiving-operations: Record arrivals, movements, and deliveries."
user-invocable: false
---

# Intake Recording

| | |
|---|---|
| **ID** | `intake-recording` |
| **Used by** | [receiving-operations](../../agents/firearm-store-receiving-operations.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Record arrivals, movements, and deliveries.

## Approach

1. Match each arrival against the purchase transaction report: items, quantities, serial numbers.
2. Record condition notes and the documents received.
3. New item records start `on-hold` (see `received-items-unavailable`).
4. For deliveries to a customer, record a movement of type `delivery` with the case ID.
5. Report mismatches and damage instead of correcting them.

## Watch Out

- Never mark a received item available.

This skill is the source of truth for this guide.
