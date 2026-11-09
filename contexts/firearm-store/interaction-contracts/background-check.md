# Background Check

| | |
|---|---|
| **ID** | `background-check` |
| **Requester** | [compliance-orchestrator](../../../.claude/agents/firearm-store-compliance-orchestrator.md) |
| **Provider** | [government-background-check](../../../.claude/agents/firearm-store-government-background-check.md) |

## Purpose

Compliance asks the government service whether the customer has previous criminal records.

## Input

- Case ID.
- Customer identifying information from the case.

## Expected Output

Fictional background-check result.

## Outcomes

- `no-record`
- `record-found` (mock record summary)
- `delayed`
- `cannot-determine`

## Rules

- All records are mock and every result is labeled fictional; it is never a real eligibility determination.
- External party: compliance always validates the response.
