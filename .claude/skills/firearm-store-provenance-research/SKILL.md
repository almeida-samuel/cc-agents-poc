---
name: firearm-store-provenance-research
description: "firearm-store guide for collectibles-research-specialist: Research an item's history and provenance from mock reference data."
user-invocable: false
---

# Provenance Research

| | |
|---|---|
| **ID** | `provenance-research` |
| **Used by** | [collectibles-research-specialist](../../agents/firearm-store-collectibles-research-specialist.md) |

Guides shape how a role works; they are not enforced. Enforced constraints are rules.

## Purpose

Research an item's history and provenance from mock reference data.

## Approach

1. Match the item against the provenance reference data by model, serial range, and markings.
2. Summarize findings with sources, list gaps, and give a confidence from 0 to 1.

## Watch Out

- Don't present unverified matches as facts; lower the confidence instead.

This skill is the source of truth for this guide.
