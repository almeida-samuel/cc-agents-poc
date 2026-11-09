# Claude Code Runtime

Claude Code is the POC's runtime. To keep a **single source of truth**, the files under `.claude/` *are* the role, guide, rule, and protocol definitions; nothing is generated or copied. Shared context definitions stay in `contexts/<context>/`.

## Run a Case

```text
/run-case firearm-store A customer wants a quotation for a handgun, paying cash
```

The main conversation becomes the **dispatcher** and the **human boundary**. You are the operator: case responses and human requests appear in the conversation, and you answer there.

## Where Each Definition Lives

| Framework concept | File |
|---|---|
| Case protocol (folders, tasks, outputs, events) | `.claude/skills/case-protocol/SKILL.md`, preloaded into every agent |
| Dispatcher loop + human boundary | `.claude/skills/run-case/SKILL.md`, run by the main conversation |
| Orchestrator turn procedure, case response, human request | `.claude/skills/orchestrator-turn/SKILL.md`, preloaded into every orchestrator |
| Rule | `.claude/rules/<context>/<rule-id>.md`, path-scoped to `data/<context>/**`: loaded automatically when an agent touches that context's data. Agents also list their binding rules as a fallback |
| Role (orchestrator, specialist, external party): spec + expected outputs | `.claude/agents/<context>-<role-id>.md` |
| Guide | `.claude/skills/<context>-<guide-id>/SKILL.md`, preloaded into the roles that use it |
| Human role | `contexts/<context>/human-roles/<role-id>.md`, no agent; reached through the human boundary |
| Units, contracts, organization, mock data | `contexts/<context>/` (each agent lists the contracts it must read) |
| Operator | The user |

Agent and skill names are prefixed with the context so several contexts can coexist.

## Design Choices

- **One dispatcher invokes every agent.** Claude Code subagents can start their own subagents, but role agents don't get the `Agent` tool. Every invocation therefore goes through the dispatcher, which logs it as an event and keeps the protocol's "one writer per file" guarantee.
- **Tools are minimal.** Orchestrators: Read, Write, Edit, Glob, Grep. Specialists and external parties: Read, Write, Glob, Grep. No shell, no web, no MCP (rule `mock-data-only`).
- **Checks are done by agents.** There is no code. Orchestrators apply the rules' "Check" lines and acceptance criteria and record each result in `checks`.

## Keeping It in Sync

Each fact lives in one file; links connect them. When something changes, update both ends of its links:

- **New role**: new agent file (copy a sibling of the same kind) or, for a human, a `human-roles/` file. Add it to its unit file, `organization.md`, and the rules and contracts that involve it.
- **New guide**: new skill folder; add it to the `skills:` list and Guides section of each agent that uses it.
- **New rule**: new file in `.claude/rules/<context>/` (with the `paths` frontmatter) listing the roles it binds; add it to each of those agents' Rules section and "Shared Files to Read".
- **New contract**: new file in `interaction-contracts/`; add it to the requester's and provider's Collaboration Interfaces and "Shared Files to Read", and to the interaction matrix in `organization.md`.
