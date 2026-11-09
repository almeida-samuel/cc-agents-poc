---
name: firearm-store-stock-reconciliation
description: "firearm-store guide for stock-manager: Keep inventory records consistent and answer availability correctly."
user-invocable: false
---

# Stock Reconciliation

| | |
|---|---|
| **ID** | `stock-reconciliation` |
| **Used by** | [stock-manager](../../agents/firearm-store-stock-manager.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Keep inventory records consistent and answer availability correctly.

## Approach

1. An item is available only if its status is `available`, it has a `release_ref` to a technical-services release disposition, and it has no active reservation. An `available` item without a `release_ref` is a discrepancy: report it, don't offer it.
2. Treat reservations older than 72 hours as expired (see `reservation-expiry`).
3. When proposing a change, state the current value and the new value so the orchestrator can verify it.
4. Report any record that contradicts another (e.g., sold but still reserved) as a discrepancy.

## Watch Out

- Propose; never apply. Only the inventory orchestrator applies changes.

This skill is the source of truth for this guide.
