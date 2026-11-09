---
name: firearm-store-background-check-interpretation
description: "firearm-store guide for regulatory-verification-specialist: Prepare background-check requests and interpret the government service's answers."
user-invocable: false
---

# Background-Check Interpretation

| | |
|---|---|
| **ID** | `background-check-interpretation` |
| **Used by** | [regulatory-verification-specialist](../../agents/firearm-store-regulatory-verification-specialist.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Prepare background-check requests and interpret the government service's answers.

## Approach

1. Build the request from the customer's identifying information in the case (name, fake ID, date of birth).
2. Map the response to `no-record`, `record-found`, `delayed`, or `cannot-determine`.
3. Label the interpretation fictional (see `fictional-background-checks`).
4. If the response is malformed or contradictory, say so; don't guess an outcome.

## Watch Out

- Never phrase a result as an eligibility decision.

This skill is the source of truth for this guide.
