---
name: orchestrator-turn
description: How a unit or cross-unit orchestrator handles one turn of a case - validate subtask outputs, apply changes within authority, delegate, escalate, or respond. Preloaded by every orchestrator agent.
user-invocable: false
---

# Orchestrator Turn

This skill is the source of truth for an orchestrator's turn. File layout and the output format are in the `case-protocol` skill.

An orchestrator is invoked once when its task starts, then again (a `resume` task) each time all the subtasks it created have finished. On every turn:

1. **Read**: your role spec (your agent file), the case `request.md`, your `state/<unit>.json` (create it on your first turn), and the `output.json` of every subtask you created that has finished (`tasks.json` shows which).
2. **Validate** each subtask output against its expected output's acceptance criteria and every rule binding it. Record each in `checks`. Outputs from external parties are always validated (rule `validate-external-outputs`).
3. **Rejected**: add a `new_tasks` retry with `attempt` + 1 and what to fix. If the output already had `attempt` 4 (3 retries), escalate instead.
4. **Accepted**: apply the changes it implies, **only** within the authority listed in your role spec, to your state and records. List each in `state_changes`.
5. **Next**: either
   - create the next subtasks or contract requests (`new_tasks`, with `depends_on` when one needs another; independent ones run in parallel), or
   - escalate (`escalations`) anything outside your authority or matching an escalation trigger, or
   - raise a `human_requests` entry when a rule requires a person, or
   - finish: write your expected output in `output`, and if you are the entry unit and the case is answered, the `case_response`.

Collect everything the operator must hear into **one** case response (deliverable, blocking findings, and information needed together), so the operator isn't asked several times.

Never call agents directly: list them in `new_tasks` and the dispatcher runs them.

## Delegate the Work

**Never do a specialist's work yourself, even when the answer looks obvious.** If a specialist in your unit has an expected output that covers the work (for example, an availability answer, a payment status proposal, a ledger entry proposal, a movement record proposal, a completeness report), create a task for that specialist. Your turns only plan, validate, apply, escalate, and respond.

- Every change you apply to your unit's **records** must cite, as its `source`, an accepted output of a specialist or an external party. A contract request alone is a valid source only where your role spec's authority says you apply it directly (e.g., inventory applying a transaction report).
- Changes to your own `state/<unit>.json` (tracking what you asked and what came back) need no specialist.
- Record a `delegation` check for every record change: `pass` when its source is an accepted specialist or external output (or a directly applicable contract request), `fail` otherwise. A `fail` means you must create the specialist task instead of applying the change.
- The cross-unit orchestrator has no specialists; this section doesn't apply to it.

## Case Response and Human Request

Produced by orchestrators: the case response by the entry unit's orchestrator, human requests by any orchestrator. An output that fails its acceptance criteria is sent back; after 3 retries it is escalated.

### Case Response

**Goes to:** Operator, through the human boundary (sent by the entry unit's orchestrator)

| Field | Type | Required | Description |
|---|---|---|---|
| `case_id` | string | yes | Case the output belongs to. |
| `entry_unit` | string | yes | `sales` or `purchasing`. |
| `type` | enum | yes | `deliverable`, `blocking-findings`, or `information-needed`. |
| `summary` | text | yes | One or two sentences the operator can act on. |
| `deliverable` | object | no | For `deliverable`: e.g., the quote or the used-item offer. |
| `findings` | list | no | For `blocking-findings`: `{unit, finding, source_ref}` per finding. |
| `information_needed` | list | no | For `information-needed`: `{field, reason, rule_id}` per item. |
| `next_step` | text | yes | What happens when the operator answers or accepts. |

**Acceptance criteria**

- Exactly one `type`, with its matching field filled.
- If anything blocks the case and information is also missing, use `blocking-findings` and list the missing information too, so the operator gets everything at once.
- Every finding cites the contract response or event it comes from.
- Every information-needed item names the rule that requires it.
- Background-check outcomes appear as their outcome code and are labeled fictional.

### Human Request

**Goes to:** Operator or a human role, through the human boundary

| Field | Type | Required | Description |
|---|---|---|---|
| `case_id` | string | yes | Case the output belongs to. |
| `addressed_to` | string | yes | `operator` or a human role ID (e.g., `inspection-technician`). |
| `kind` | enum | yes | `approval`, `judgment`, `physical-action`, or `external-information`. |
| `what` | text | yes | What is needed. |
| `why` | text | yes | Why it is needed, with references to the case. |
| `options` | list | no | `{option, consequence}`; required for `approval` and `judgment`. |
| `blocking_task` | string | yes | Task waiting in `awaiting-human`. |

**Acceptance criteria**

- `approval` and `judgment` requests list at least two options, each with its consequence.
- `physical-action` requests name the item and what to check.
- Understandable without reading the whole case.
