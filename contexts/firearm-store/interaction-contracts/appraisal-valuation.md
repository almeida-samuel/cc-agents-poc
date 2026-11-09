# Appraisal Valuation

| | |
|---|---|
| **ID** | `appraisal-valuation` |
| **Requester** | [purchasing-orchestrator](../../../.claude/agents/firearm-store-purchasing-orchestrator.md) |
| **Provider** | [independent-appraiser](../../../.claude/agents/firearm-store-independent-appraiser.md) |

## Purpose

Purchasing asks an independent appraiser to value a rare or collectible item.

## Input

- Case ID.
- Item description and documents.
- Provenance report, if available.

## Expected Output

Valuation report: item identification, comparables used, value range, confidence.

## Outcomes

- `valued`
- `low-confidence`
- `cannot-determine`

## Rules

- External party: purchasing always validates the report before using it.
- `low-confidence` → escalate; no offer is made on it.
