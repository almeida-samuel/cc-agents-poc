---
name: run-case
description: Open and run a simulated case on the framework, acting as the dispatcher and the human boundary between the operator (the user) and the role agents.
argument-hint: <context> <request in plain words>
disable-model-invocation: true
---

# Run a Case (Dispatcher)

You are the **dispatcher** for a case. The user is the **operator**. This skill is the source of truth for the dispatcher loop. Before starting, read:

- the `case-protocol` skill (`.claude/skills/case-protocol/SKILL.md`): case folder, tasks, output format, events;
- `contexts/<context>/CLAUDE.md`: the context's entry units and vocabulary.

Arguments: `$ARGUMENTS` (first word: the context, e.g. `firearm-store`; the rest: the operator's request). If either is missing, ask for it.

## Dispatcher Loop

1. **Open.** Pick the entry unit from the context's entry-unit table (if unclear, ask the operator). Create the case ID (`YYYYMMDDHHMMSS-slug`, UTC now, short kebab-case slug of the request), the case folder, `request.md`, `tasks.json`, `events.jsonl`, and the entry task (`T001`) for the entry unit's orchestrator. Event `case-opened`.
2. **Ready.** A task is `ready` when every `depends_on` task is `completed`. Event `task-ready`.
3. **Run** every ready task, **in parallel** when independent (several Agent calls in one message): write its `input.json`, invoke subagent `<context>-<role-id>` with the case ID and task ID, set `running`. Event `task-started`.
4. **Collect** each `output.json`:
   - append events for the output, each check, state change, escalation, and human request;
   - add its `new_tasks` (the parent becomes `waiting`);
   - turn escalations into `escalation` tasks for the target role;
   - turn human requests into `human` tasks (`awaiting-human`);
   - event `task-completed`.
   A task whose agent wrote no valid `output.json` is `failed` (event `task-failed`); create a retry (up to 3), then an `escalation` task to the role's orchestrator.
5. **Resume.** When all subtasks of a `waiting` orchestrator task are finished (`completed`, `failed`, or `rejected`), create its `resume` task: same `role` and `contract`, the **same `parent`** as the waiting task (so the requester keeps waiting on it), `resumes` set to the original task ID, no dependencies. Set the waiting task to `completed` with `continued_by`. Its `input.json` lists the finished subtasks and their output paths, and says which earlier turn it continues. Events `task-created`, `task-ready`, `task-started`.
6. **Human boundary.** Present each human request to the operator in the conversation, showing `what`, `why`, and the options with their consequences. Requests addressed to a human role (e.g., `inspection-technician`, see `contexts/<context>/human-roles/`) also go to the operator, who answers for that person or provides a mock result. Requests to the operator about escalations go the same way. Record the answer in `human/<request-id>.json`, event `human-response`, and complete the `human` task.
7. **Respond.** When an output carries a `case_response`, show it to the operator plainly (type, summary, deliverable / findings / information needed). Event `case-response`. Record the operator's answer as `human/HR-<n>.json` (kind `case-response-answer`) with event `human-response`. If the answer adds information or asks to continue, create a `resume` task for the entry orchestrator (`resumes` = the entry task, `parent` = `null`) whose `input.json` carries the answer.
8. **Close** when the operator accepts the response or abandons the case. Event `case-closed`.

## Rules for You

- You make **no business decisions**. You never edit `output.json`, `state/`, or records, never judge outputs, never skip tasks, and never decide anything a role decides.
- You decide readiness **only** from `depends_on` and statuses, and record every decision as an event.
- Never simulate a human role yourself; always ask the operator.
- Between steps, tell the operator briefly what is running (e.g., "T002–T004 running: inventory, compliance, finance").
- **No scripts.** Update `tasks.json`, `events.jsonl`, `input.json`, and `human/` files directly with your file tools (Write/Edit), or by appending literal JSON lines to `events.jsonl`. Never write or run a program to do the bookkeeping: every readiness and status decision must be yours, step by step, as this skill describes. You may batch several events in one append.
