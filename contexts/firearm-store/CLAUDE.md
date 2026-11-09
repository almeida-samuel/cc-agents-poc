# Firearm Store Context

Guidance for the **firearm-store** context: a mock firearm-retail store running on the framework described in the root `CLAUDE.md`. Use the framework vocabulary (units, roles, contracts, rules, guides, expected outputs, cases); this file maps it to the store's own terms.

- [Current State](#current-state)
- [Overview](#overview)
- [Vocabulary Mapping](#vocabulary-mapping)
- [Safety Boundaries](#safety-boundaries)
- [Inspection and Quality Control](#inspection-and-quality-control)
- [Organizational Map](#organizational-map)
- [Roadmap and Future](#roadmap-and-future)

## Current State

Design phase. The **organizational map** is drafted (see [Organizational Map](#organizational-map)) and needs review. No workflows, simulated transactions, or software yet.

## Overview

A full firearm store, including external parties, chosen as the first context because it offers a wide variety of roles and interactions: front-of-house sales, background checks, financing of expensive purchases, inventory, compliance, purchasing, and external experts. The store handles both new and used stock of all types.

Example scenario: a customer brings in a rare firearm to sell, and the store calls an external expert to appraise it. That expert is also an agent, which tests delegation, permissions, and trust boundaries.

This is an agent-behavior experiment, not a production firearms-commerce system.

## Vocabulary Mapping

| Framework | Firearm store |
|---|---|
| Unit | Department |
| Operator | Owner / CEO (in general a human; orchestrates cases and is the top authority) |
| Case | Order, acquisition, inspection, or inquiry (e.g., `20261106192210-steve-rogers-orders-shield`). Customers and sellers are information in the case request, not roles |
| Entry unit | Sales for buying and quotations; purchasing for selling a used item and restocking |
| Cross-unit orchestrator | General manager |
| External party | Government background-check service, appraisers, external technical specialists, suppliers |
| Human | Qualified inspection technician, and any human review |

Escalation chain: specialist → department orchestrator → general manager orchestrator → operator (owner/CEO), or a human at any point a rule requires it.

## Safety Boundaries

Not negotiable. They are defined in two rules, which load automatically whenever an agent works on files under `data/firearm-store/`:

- [mock-data-only](../../.claude/rules/firearm-store/mock-data-only.md): all data is mock; no real transactions, regulatory determinations, physical testing by AI, or live services.
- [fictional-background-checks](../../.claude/rules/firearm-store/fictional-background-checks.md): the government service holds fictional criminal records only; results are never eligibility determinations.

## Inspection and Quality Control

- Quality control covers **both new and used** incoming stock. Purchasing or receiving requests inspections.
- Used items may have unknown maintenance history and safety issues, so they must be inspected before they can be accepted for resale.
- Condition assessment (an agent) reviews documentation and maintenance history. A **qualified human technician** performs the physical safety inspection outside the agent system, reached through a physical-action request at the human boundary; the simulation records **mock inspection results only**.
- Inspection outcomes: pass, needs work, reject, or inconclusive. Needs work or reject blocks inventory release and can trigger supplier relations or human review.
- **Never release inventory on an inconclusive result.** Inconclusive results block resale and escalate to the QA manager or human review. The QA manager may request a new inspection or reject the item, but may not release it on the inconclusive result.

## Organizational Map

The organizational map (draft) lives in separate files. Keep this file short; put details there.

- `organization.md`: organization diagram, role-kind summary, interaction matrix, escalation paths, handoff scenarios, and open questions. Start here.
- `units/<unit>.md`: one per department: orchestrator, members, and the records it owns.
- Roles: each non-human role is a Claude Code agent, `.claude/agents/firearm-store-<role-id>.md`, holding its full spec and expected outputs. Human roles are in `human-roles/`.
- `interaction-contracts/<contract-id>.md`: requester, provider, input, expected output, outcomes, and rules.
- Rules: `.claude/rules/firearm-store/<rule-id>.md`: enforced constraints: scope, the roles each binds, condition, consequence, and the check. Path-scoped to `data/firearm-store/**`, so Claude Code loads them whenever an agent touches case data or records.
- Guides: each is a Claude Code skill, `.claude/skills/firearm-store-<guide-id>/SKILL.md` (not enforced).
- Expected outputs: inside each role's agent file. The case response and human request are in the `orchestrator-turn` skill.
- `test-scenarios.md`: `/run-case` commands with expected results, and an audit checklist for each run.
- `mock-data.md`: everything that must be mocked (inventory, credit profiles, criminal records, and so on), its owner, and its location.

Conventions used across these files:

- Requesters and providers of contracts are always orchestrators or external parties. Specialists prepare request content and interpret responses.
- Only a unit's orchestrator changes the records that unit owns.
- Every sale and purchase is reported to both finance and inventory (`transaction-report`). A sale counts as completed, and is reported, only when payment is `paid` **and** the order is `delivered`.
- Any gun with a full-auto mode requires the customer's address (compliance requirement).
- The operator delegates nothing to the general manager at the start; every exception reaches the operator.
- Each agent file lists its guides (`skills:` frontmatter and Guides section), its binding rules (Rules section and "Shared Files to Read", as a fallback to automatic loading), and the contracts it uses. Rule files list the roles they bind. Keep both sides in sync when adding or renaming a role, guide, rule, or contract.
- All of these are Markdown with a fixed structure for now; the real file formats are still an open framework question.
- Each fact lives in one file: edit it there, and keep the structure consistent with sibling files. There are no generator scripts.

## Roadmap and Future

**Next task:** review the organizational map draft (including rules, guides, and expected outputs) with the user.

**Open questions (ask the user):**
- Specific mock scenarios and test cases.
- Which unusual situations need dynamic extensions (e.g., an out-of-state customer).
