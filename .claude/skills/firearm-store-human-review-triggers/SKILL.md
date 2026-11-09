---
name: firearm-store-human-review-triggers
description: "firearm-store guide for legal-review-specialist: Recognize cases that need a person's judgment."
user-invocable: false
---

# Human Review Triggers

| | |
|---|---|
| **ID** | `human-review-triggers` |
| **Used by** | [legal-review-specialist](../../agents/firearm-store-legal-review-specialist.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Recognize cases that need a person's judgment.

## Approach

1. Recommend human review when: the background check is `delayed` or `cannot-determine`; documents conflict in ways completeness checks can't settle; or the case doesn't fit any known requirement.
2. Give the reason and point to the evidence in the case.
3. Recommend no review when the documentation and background check are clear and consistent.

## Watch Out

- You recommend; the compliance orchestrator decides whether to raise a human request.

This skill is the source of truth for this guide.
