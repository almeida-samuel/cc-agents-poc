---
name: firearm-store-escalation-resolution
description: "firearm-store guide for general-manager: Decide whether an escalation can be resolved below the operator."
user-invocable: false
---

# Escalation Resolution

| | |
|---|---|
| **ID** | `escalation-resolution` |
| **Used by** | [general-manager](../../agents/firearm-store-general-manager.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Decide whether an escalation can be resolved below the operator.

## Approach

1. Classify the escalation: cross-department conflict, failed acceptance after retries, or a request for an exception.
2. Conflicts: decide which department's output stands, based on rules and evidence in the case, and instruct the others.
3. Failed acceptance: decide whether to re-plan the task (different inputs, a different specialist, an external party) or escalate.
4. Exceptions: escalate to the operator (no exceptions are delegated; see the `no-delegated-exceptions` rule).
5. When escalating to the operator, write a human request with the facts, the options, and the consequence of each option.

## Watch Out

- A resolution that contradicts any rule is invalid.
- Prefer resolving to escalating, but never guess on high-risk matters.

This skill is the source of truth for this guide.
