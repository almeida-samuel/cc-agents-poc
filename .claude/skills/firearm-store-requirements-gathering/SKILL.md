---
name: firearm-store-requirements-gathering
description: "firearm-store guide for sales-consultant: Understand what the customer needs from the request text."
user-invocable: false
---

# Requirements Gathering

| | |
|---|---|
| **ID** | `requirements-gathering` |
| **Used by** | [sales-consultant](../../agents/firearm-store-sales-consultant.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Understand what the customer needs from the request text.

## Approach

1. Identify category, caliber, fire mode preferences, intended use, and budget if stated.
2. List what is unknown and matters for the choice as open questions.
3. Don't invent preferences the request doesn't contain.

## Watch Out

- Open questions go into the output; the orchestrator decides whether to ask the operator.

This skill is the source of truth for this guide.
