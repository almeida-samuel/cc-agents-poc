# External Technical Assessment

| | |
|---|---|
| **ID** | `external-technical-assessment` |
| **Requester** | [technical-services-orchestrator](../../../.claude/agents/firearm-store-technical-services-orchestrator.md) |
| **Provider** | [external-technical-specialist](../../../.claude/agents/firearm-store-external-technical-specialist.md) |

## Purpose

Technical services asks for an independent assessment when internal results conflict or need a second opinion.

## Input

- Item documents.
- Condition assessment.
- Inspection results.

## Expected Output

Independent assessment with a confidence level.

## Outcomes

- `assessed`
- `low-confidence`
- `cannot-determine`

## Rules

- External party: technical services always validates the assessment.
- An external assessment cannot turn an `inconclusive` physical result into a pass; a new physical inspection is required.
