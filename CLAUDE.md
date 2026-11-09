# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

This file covers the **framework**. The case protocol (how agents read and write cases) is the `case-protocol` skill (`.claude/skills/case-protocol/SKILL.md`); the dispatcher loop is the `run-case` skill and an orchestrator's turn is the `orchestrator-turn` skill. Each context that runs on the framework has its own guidance file at `contexts/<context>/CLAUDE.md`. When working on a context, read its file as well. Contexts so far:

- [Current State](#current-state)
- [Project Overview](#project-overview)
- [Framework Concepts](#framework-concepts)
  - [Framework vs. Context](#framework-vs-context)
  - [Context Definition](#context-definition)
  - [Rules, Guides, and Expected Outputs](#rules-guides-and-expected-outputs)
  - [Operator](#operator)
  - [Role](#role)
  - [Authority](#authority)
  - [Escalation](#escalation)
  - [Human Boundary](#human-boundary)
  - [Interaction Contract](#interaction-contract)
  - [Case, Task, and Event](#case-task-and-event)
  - [Template](#template)
- [Architecture and Technology Stack](#architecture-and-technology-stack)
  - [Runtime](#runtime)
  - [Persistence](#persistence)
  - [Execution Model](#execution-model)
- [Code Style & Patterns](#code-style--patterns)
  - [Conventions](#conventions)
- [Development Workflow](#development-workflow)
  - [The Pipeline](#the-pipeline)
  - [The Technical Boundary](#the-technical-boundary)
- [Roadmap and Future](#roadmap-and-future)

## Current State

Design phase. There is no code, build, lint, or test tooling, only Markdown. **No programming language will be used**: the framework is executed by agents and skills following Markdown instructions and reading/writing case files. Keep framework knowledge in this file. Each context keeps a short guidance file (`contexts/<context>/CLAUDE.md`) plus its definition files (units, roles, contracts, and so on) next to it.

The [Framework Concepts](#framework-concepts) are a **draft** to be reviewed with the user.

**Do not** create workflows, execute simulated cases, or implement software until the user explicitly changes scope.

## Project Overview

A proof of concept for a **framework in which agents run an entire operation, with humans at the boundary** wherever they are needed (approval, judgment, physical action, real-world information). It explores delegation, orchestration, collaboration across units, escalation, and structured artifacts.

The framework is **context-agnostic**. It has no notion of a business, company, or any particular kind of organization. Whatever runs on it (a store, a software team, a lab, anything else) is a **context**: a pluggable definition package. Nothing context-specific belongs in the framework, and adding a context must never require changing the framework.

The framework is also:

- **Operator-agnostic**: whatever uses the framework is an abstract [operator](#operator). The framework makes no assumptions about what or who the operator is (in general it is a human). People otherwise appear only as abstract [human](#role) roles. Both are reached through the [human boundary](#human-boundary).
- **Runtime-agnostic in concept**: its concepts make no assumptions about which agent platform executes it. In this POC, though, the role, guide, and protocol definitions are written directly as Claude Code agents and skills (see [Runtime](#runtime)).

All data in the POC is mocked. No integrations with live services.

Design priorities: settle concepts and organization (roles and how they interact) before designing any workflow. Aim for variety of interactions, not a large number of roles.

## Framework Concepts

Status: **draft**. This section defines vocabulary and responsibilities only. Schemas, file formats, and runtime mechanics are open (see [Roadmap and Future](#roadmap-and-future)). Use this vocabulary consistently. Context-specific terms ("department", "order", "customer", "CEO", and so on) may appear only as examples of how a context names a framework concept, never as framework concepts themselves.

### Framework vs. Context

| Layer | Owns |
|---|---|
| **Framework** | The definitions of the concepts in this section: what the operator, a unit, role, contract, rule, guide, expected output, case, task, event, and template are, plus authority, escalation, and the human boundary |
| **Context definition** | Concrete instances of those concepts for one context: its units, roles, interaction contracts, rules, guides, expected outputs, and templates |
| **Runtime adapter** | Binding framework concepts and context definitions to one agent platform. In this POC, the Claude Code adapter (`.claude/`) also *holds* the role, guide, and protocol definitions |
| **Case data** | Mock state produced while running a context: `data/<context>/cases/<case-id>/` |

Context definitions depend on the framework, and case data depends on its context. Because roles and guides live in `.claude/`, the context's shared files (units, contracts, rules, organization) link to the agent and skill files, and agents read the context's shared files.

### Context Definition

A context is a set of declarative definitions (format TBD):

- **Entry units**: which unit receives each kind of case request and assembles the case response.
- **Units**: groups of roles, each with one orchestrator. A context maps units to its own terms (departments, teams, squads...). A context with more than one unit also defines a **cross-unit orchestrator** (see [Role](#role)).
- **Roles**: one spec per role (see [Role](#role)).
- **Interaction contracts**: how roles in different units ask each other for work.
- **Rules**, **guides**, and **expected outputs**: the extension points (below).
- **Templates**: fixed sequences of tasks for a kind of case (later stage).

### Rules, Guides, and Expected Outputs

These are the three **extension points** for adding context knowledge. They can be attached at the context, unit, role, or contract level. A narrower scope adds to a broader one and must not contradict it.

- **Rules**: constraints that must hold. A rule states a condition and the consequence of breaking it (block, reject, escalate). Written as explicit, mechanical checks (the "Check" line) that an agent applies to the case files, not as open-ended prose. *Example: a task whose verification result is inconclusive must not be marked complete; escalate to the unit orchestrator.*
- **Guides**: instructions and domain knowledge that tell a role how to do its work well. They influence agent reasoning but are not enforced. In this POC each guide is a Claude Code skill, preloaded into the roles that use it. *Example: how to research and compare reference sources for an assessment.*
- **Expected outputs**: the output contract for a task: the artifact or data it must produce, its structure, and **acceptance criteria**. An orchestrator uses the acceptance criteria to decide whether to accept a result and move on without escalating. *Example: an assessment report must include the subject, sources used, a result, and a confidence level; confidence below the threshold → escalate.*

### Operator

The operator is whatever uses the framework to run a context. It is outside the agent system and, in general, a human. The operator:

- **orchestrates cases**: opens each one with a **case request** (e.g., "a quotation for X"), sees its progress, and receives anything blocked, escalated, or awaiting a human;
- receives a **case response** from the case's entry unit: the requested deliverable, the findings that block it, or the information still needed. When information is needed, the operator answers and the case resumes, so a case can go back and forth several times before it closes;
- is the **top authority**: the last step of every escalation chain and the final decision-maker on anything no role has authority for.

The operator is not a role and is not defined by a context; a context may only give it a local name (e.g., "owner"). It is reached through the [human boundary](#human-boundary).

### Role

A role is a responsibility, not a person. One agent may fill several roles, and one role may be filled by several agent invocations.

| Kind | Description |
|---|---|
| **Orchestrator** | Supervises a unit. Delegates tasks, validates outputs against expected outputs, and is the only role kind that changes state (case state and shared records), within its authority. Escalates otherwise. An entry unit's orchestrator also sends the case response. |
| **Cross-unit orchestrator** | An orchestrator whose scope spans several units. It knows how the units fit together, coordinates work and resolves conflicts and escalations that involve more than one unit, so those matters don't need the operator. Required whenever a context has more than one unit. Escalates to the operator only what is outside its authority. |
| **Specialist** | Does one focused kind of work and returns an output to its orchestrator. Never changes case state or shared records; when a change is needed, its output proposes it and the orchestrator applies it. |
| **External party** | Someone outside the context's own units (a provider, an independent expert), simulated by an agent. Less trusted: the requesting orchestrator always validates its outputs. |
| **Human** | A function only a person may perform. Reached through the [human boundary](#human-boundary). Never simulated by an agent unless the run explicitly mocks the human. |

Every role spec defines: **kind, mission, scope, authority, required inputs, produced outputs (expected outputs), guides, dependencies, escalation triggers, reporting line, collaboration interfaces (contracts)**, plus any rules attached to the role.

### Authority

Authority is the explicit list of state changes an orchestrator may make on its own. The operator may delegate extra authority to the cross-unit orchestrator; by default it delegates nothing (e.g., "mark a review task complete", "reserve a resource"). Anything not listed requires escalation. Specialists, external parties, and humans have no state-changing authority; they return outputs or responses, and the orchestrator applies any change once it accepts them. Orchestrators have **bounded autonomy**: if a specialist's output meets the acceptance criteria and no rule is violated, the orchestrator applies it and advances the case without asking.

### Escalation

An escalation is raised when:

- an action is outside the role's authority,
- an output still fails acceptance criteria after **3 retries**,
- a rule says to escalate,
- the result is ambiguous, conflicting, or inconclusive,
- the matter is high-risk or needs physical verification.

Escalations go up the reporting line (specialist → unit orchestrator → cross-unit orchestrator → operator) or straight to the human boundary when a rule or role kind requires a person. Each escalation is recorded as an event with its reason, and the affected task stays in the `escalated` state until it is resolved.

### Human Boundary

The single gateway between the agent system and anything outside it that is not an external party: the [operator](#operator) and human roles. Case requests come in and case responses go out through it. A **human request** is written to the case and addressed to either the operator or a human role: what is needed, why, which options exist, and what happens with each answer. The affected task waits in the `awaiting-human` state; the human's response is recorded as an event and the case resumes. Request kinds:

- **Approval**: authorize something beyond agent authority.
- **Judgment**: resolve ambiguity agents cannot settle.
- **Physical action**: something done in the real world; the human records the result.
- **External information**: facts available only outside the system.

In the POC, human responses are mocked or entered by hand.

### Interaction Contract

A request/response agreement between two roles, usually across units: who may ask, the input it requires, the expected output it returns, the possible outcomes (including failure and "cannot determine"), and the rules that apply. Units interact **only** through contracts and escalations, which keeps them self-contained.

### Case, Task, and Event

- **Case**: a piece of work a context processes (whatever the context calls it: an order, a ticket, a request). The operator opens each case with a case request; people or organizations mentioned in it (e.g., a customer) are information, not roles. Each case has its own folder holding current state, an append-only event log, input documents, and produced artifacts. The case ID (and folder name) is a UTC timestamp (`YYYYMMDDHHMMSS`) followed by a human-readable kebab-case slug, e.g., `20261106192210-steve-rogers-orders-shield`. State always lives in files, never in agent memory, so any agent invoked later can pick up the case.
- **Task**: one step of a case, assigned to a role, with declared dependencies. Candidate states: `pending`, `ready`, `running`, `blocked`, `awaiting-human`, `escalated`, `completed`, `rejected`, `failed`. Blocked, escalated, and awaiting-human tasks must be visible to the operator.
- **Event**: an append-only record of something that happened (task started/completed, output accepted/rejected, rule triggered, escalation raised/resolved, human request/response, state change). The event log is the audit trail; the state file is a view of it.

### Template

A fixed, reusable plan of tasks and dependencies for a kind of case. Workflows start as fixed templates because most steps repeat with minor differences. Later, orchestrators may make **controlled dynamic extensions** (inserting a small extra step for an unusual situation), recorded as events.

## Architecture and Technology Stack

### Runtime

The framework's concepts don't depend on any agent platform. For this POC the only runtime is **Claude Code**, and to keep a **single source of truth** its files are the definitions themselves, not copies:

- `.claude/agents/<context>-<role-id>.md`: one agent per non-human role, holding its full role spec and expected outputs.
- `.claude/skills/<context>-<guide-id>/`: one skill per guide, holding the full guide.
- `.claude/skills/case-protocol/`, `run-case/`, `orchestrator-turn/`: the case protocol, the dispatcher loop, and the orchestrator turn (including the case response and human request).
- `.claude/rules/<context>/`: one rule file per context rule, path-scoped to `data/<context>/**` so it loads whenever an agent touches that context's case data or records.

Shared context files (units, contracts, organization, mock data, human roles) stay in `contexts/<context>/`. Moving to another runtime later means converting the `.claude/` files. `runtimes/claude-code/README.md` describes the mapping.

### Persistence

The local filesystem is the database, kept as inspectable folders. Layout (everything exists except `data/`, which is created by mock data and by running cases):

```text
CLAUDE.md                   # framework guidance (this file)
contexts/
  <context>/
    CLAUDE.md               # context guidance
    organization.md         # org diagram, interaction matrix, escalation paths
    mock-data.md            # what must be mocked, its owner and location
    units/
    human-roles/            # human roles (no agent)
    interaction-contracts/
    templates/              # not written yet
data/
  <context>/
    records/<unit>/         # shared records, changed only by the unit's orchestrator
    external/<party>/       # mock data behind each external party
    cases/
      <case-id>/
        request.md
        tasks.json
        events.jsonl
        state/<unit>.json
        tasks/<task-id>/{input,output}.json
        human/
        documents/
        artifacts/
runtimes/
  claude-code/README.md     # how .claude/ maps to the framework
.claude/
  agents/                   # one per non-human role: spec + expected outputs
  skills/                   # guides, case-protocol, run-case, orchestrator-turn (+ case response, human request)
  rules/<context>/          # context rules, path-scoped to data/<context>/**
```

### Execution Model

- Agents are invoked on demand, never kept running.
- An invocation reads the case files, does its task, writes outputs and events, and exits.
- When the operator opens a case, determine which tasks can run in parallel and which depend on others. Independent tasks run in parallel; dependent tasks wait for prerequisites.
- Deciding which tasks are ready follows declared dependencies and task states only. It is performed by an agent following strict instructions, not by code, and each decision is recorded as an event so it can be audited.
- Rule checks and acceptance-criteria checks are also performed by agents, and each result is recorded as an event.

## Code Style & Patterns

### Conventions

- Framework concepts stay context-agnostic. Anything specific to a context (vocabulary, units, roles, contracts, rules, guides, expected outputs, templates) belongs in `contexts/<context>/`, never in the framework or this file.
- Case data lives in `data/<context>/`. Nothing references a specific operator.
- Each piece of knowledge lives in exactly one file: roles, guides, rules, and protocol in `.claude/`; shared definitions in `contexts/<context>/`. Never copy content between them; link instead.

## Development Workflow

### The Pipeline

1. **Framework concepts (current, draft):** this file.
2. **Context organizational maps (current, draft):** per context, see its `CLAUDE.md`.
3. Fixed workflow templates.
4. Controlled dynamic extensions.
5. **Runtime adapters (current, draft):** `runtimes/claude-code/`, run a case with `/run-case <context> <request>`.

Do not move to a later stage without the user's direction.

### The Technical Boundary

Concepts, shared context definitions, state, and artifacts are platform-neutral. Role, guide, rule, and protocol definitions are Claude Code files (a deliberate POC trade-off for a single source of truth).

## Roadmap and Future

**Next task:** review the framework concepts and the firearm-store organizational map drafts with the user.

**Open questions (ask the user; don't decide unilaterally):**
- File formats and schemas for context definitions, state, events, and artifacts.
- How human requests and case responses are presented in the agent conversation (the operator interacts through it).
- Rules for dynamic template extensions: when an orchestrator may insert a step, and where in a template that is allowed.
- How to convert the `.claude/` definitions when moving to another runtime.
