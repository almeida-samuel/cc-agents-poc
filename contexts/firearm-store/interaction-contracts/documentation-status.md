# Documentation Status

| | |
|---|---|
| **ID** | `documentation-status` |
| **Requester** | [sales-orchestrator](../../../.claude/agents/firearm-store-sales-orchestrator.md) |
| **Provider** | [compliance-orchestrator](../../../.claude/agents/firearm-store-compliance-orchestrator.md) |

## Purpose

Sales asks whether a case's mock documentation meets the requirements for the item category and what the compliance flag is (including the background check).

## Input

- Case ID.
- Mock customer documents in the case.

## Expected Output

Documentation status (with missing fields if incomplete) and compliance flag. Verification outcomes are labeled fictional.

## Outcomes

- `clear`
- `needs-information` (lists what is missing, e.g., the customer's address)
- `check-failed` (fictional background-check record)
- `check-delayed`
- `needs-human-review`
- `cannot-determine`

## Rules

- No outcome is a legal eligibility determination.
- Sales may proceed only on `clear`; every other outcome goes into the case response.
