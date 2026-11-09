---
name: case-protocol
description: The framework's case protocol - case folder layout, who may write which file, task fields, the role output format, and events. Preloaded by every role agent; read by the run-case dispatcher.
user-invocable: false
---

# Case Protocol

Status: **draft**. How agents read and write a case. This skill is the source of truth for the protocol. The procedures that use it are in two other skills: `run-case` (the dispatcher loop) and `orchestrator-turn` (an orchestrator's turn). Concepts (case, task, event, orchestrator...) are defined in the root `CLAUDE.md`.

## Actors

- **Dispatcher**: the component that runs a case (the `run-case` skill). It is not a role and makes no business decisions. It creates and updates tasks, decides which tasks are ready (from dependencies and states only), invokes the assigned role, appends events, and carries the human boundary (presents human requests and case responses to the operator and records the answers).
- **Role agents**: one invocation per task. Each reads what it needs, writes **only** the files this protocol allows it, and returns.

## Case Folder

```text
data/<context>/cases/<case-id>/
  request.md                # the operator's case request (written by the dispatcher)
  tasks.json                # task list and statuses (dispatcher only)
  events.jsonl              # append-only event log (dispatcher only)
  state/<unit>.json         # the unit's view of the case (that unit's orchestrator only)
  tasks/<task-id>/
    input.json              # what the task must do (dispatcher, from the creating output)
    output.json             # the task's output (the assigned role only)
  human/<request-id>.json   # human request or operator answer to a case response, with the recorded response (dispatcher); IDs HR-001, HR-002, ...
  documents/                # input documents (read-only for agents)
  artifacts/                # produced artifacts (the role that produces them)
```

Shared records live in `data/<context>/records/<unit>/` and are changed **only** by that unit's orchestrator. External parties' mock data lives in `data/<context>/external/<party>/` and is read **only** by that external party.

**One writer per file.** That is what makes parallel tasks safe.

## Task

An entry in `tasks.json`:

| Field | Meaning |
|---|---|
| `id` | `T001`, `T002`, ... in creation order |
| `role` | Role ID assigned to the task |
| `type` | `work` (do the work), `contract` (serve a contract request; `contract` names it), `resume` (orchestrator continues after its subtasks finished), `escalation`, `human` |
| `parent` | Task that created it (the requester), or `null` for the entry task |
| `depends_on` | Task IDs that must be `completed` first |
| `status` | `pending`, `ready`, `running`, `waiting` (on its subtasks), `awaiting-human`, `escalated`, `completed`, `rejected`, `failed` |
| `attempt` | 1 for the first try; a retry is a new task with `attempt` + 1 (max 4: 3 retries) |
| `contract` | For `contract` tasks (and their resumes): the contract served |
| `resumes` | For `resume` tasks: the ID of the original task this turn continues (e.g., `T001`) |
| `continued_by` | On a `waiting` task once it is resumed: the ID of the `resume` task that continues it |

## Role Output

Every role writes `tasks/<task-id>/output.json`:

```json
{
  "task_id": "T004",
  "role": "stock-manager",
  "status": "completed | failed",
  "output": { },
  "new_tasks": [ ],
  "state_changes": [ ],
  "escalations": [ ],
  "human_requests": [ ],
  "case_response": null,
  "checks": [ ],
  "summary": "One line for the event log."
}
```

- `output`: the expected output, with the fields its expected-output definition lists.
- `new_tasks` (orchestrators only): `{role, type, contract?, input, depends_on?}`. Specialists in the orchestrator's own unit, or the provider orchestrator / external party of a contract the orchestrator may request.
- `state_changes` (orchestrators only): changes it **already applied** to its `state/<unit>.json` or its unit's records, each `{file, field, from, to, source}`.
- `escalations` (orchestrators only): `{to, reason, evidence}`; `to` is the next level in the reporting line.
- `human_requests` (orchestrators only): in the shape of the Human Request in the `orchestrator-turn` skill.
- `case_response` (entry-unit orchestrator only): in the shape of the Case Response in the `orchestrator-turn` skill.
- `checks`: every rule or acceptance check performed: `{check, target, result: pass|fail, detail}`.

Specialists and external parties leave every list empty except `checks`, and never write anything but their `output.json`.

## Rules for Every Role Agent

- Your task is `data/<context>/cases/<case-id>/tasks/<task-id>/`. Read its `input.json` and the case's `request.md`.
- Never write `tasks.json`, `events.jsonl`, another task's files, or anything another role owns.
- If the input is insufficient, still write `output.json`: say what is missing in `output` and `summary`; don't invent data.
- If the mock data you need doesn't exist, report it; don't make it up.

## Event

One JSON object per line in `events.jsonl`:

```json
{"ts": "2026-11-09T19:22:10Z", "actor": "sales-orchestrator", "type": "task-completed", "task_id": "T001", "data": {}}
```

Types: `case-opened`, `task-created`, `task-ready`, `task-started`, `task-completed`, `task-failed`, `output-accepted`, `output-rejected`, `check`, `state-changed`, `escalation-raised`, `escalation-resolved`, `human-request`, `human-response`, `case-response`, `case-closed`.
