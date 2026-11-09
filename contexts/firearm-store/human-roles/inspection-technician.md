# Inspection Technician

| | |
|---|---|
| **ID** | `inspection-technician` |
| **Kind** | Human |
| **Unit** | Technical services and quality assurance |
| **Reporting line** | — (answers technical services' requests through the human boundary). |

## Mission

Perform the physical safety inspection of new and used items.

## Scope

Physical checks in the real world, outside the agent system. In the simulation the result is mocked or entered by hand.

## Authority

- None. Returns outputs to its orchestrator, which applies any change it proposes.

## Required Inputs

- Physical-action request with the item and the condition assessment's recommended points.

## Produced Outputs

- Inspection result: pass, needs work, reject, or inconclusive, with notes.

Structure and acceptance criteria: see [Expected Outputs](#expected-outputs) below.

## Guides

- None.

## Dependencies

- None.

## Escalation Triggers

- None.

## Collaboration Interfaces

- Human boundary only.

## Rules

- [mock-data-only](../../../.claude/rules/firearm-store/mock-data-only.md)

Human roles have no agent: the dispatcher reaches them through the human boundary (it asks the operator).

## Expected Outputs

An output that fails its acceptance criteria is sent back; after 3 retries it is escalated.

### Inspection Result

**Goes to:** Technical services orchestrator, through the human boundary

| Field | Type | Required | Description |
|---|---|---|---|
| `item_id` | string | yes | Item. |
| `outcome` | enum | yes | `pass`, `needs-work`, `reject`, or `inconclusive`. |
| `points_checked` | list | yes | `{point, result}`. |
| `notes` | text | yes | Free text. |
| `inspected_at` | datetime | yes | When. |

**Acceptance criteria**

- Covers every recommended inspection point or explains why not.
