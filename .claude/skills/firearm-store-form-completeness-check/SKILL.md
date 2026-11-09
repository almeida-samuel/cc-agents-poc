---
name: firearm-store-form-completeness-check
description: "firearm-store guide for documentation-specialist: Check a case's documents and customer details against the mock requirements."
user-invocable: false
---

# Form Completeness Check

| | |
|---|---|
| **ID** | `form-completeness-check` |
| **Used by** | [documentation-specialist](../../agents/firearm-store-documentation-specialist.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Check a case's documents and customer details against the mock requirements.

## Approach

1. Look up the requirements for each requested item (forms and customer details).
2. Apply `full-auto-requires-address`: any item with a full-auto mode requires the customer's address.
3. List each missing or inconsistent field with the requirement that demands it.
4. Status is `complete` only when nothing is missing or inconsistent.

## Watch Out

- Check consistency too (e.g., the name on the ID matches the customer information).

This skill is the source of truth for this guide.
