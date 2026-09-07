---
name: tab-commercial-kanban
description: Run Tabtrickle commercial work through Hermes Kanban.
version: 0.1.0
author: Justin Brown (Jupdefi), Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [Kanban, Sales, Operations, Tabtrickle]
    related_skills: []
---

# Tab Commercial Kanban

Run Tabtrickle sales operations as durable, evidence-backed cards on the `tab-commercial` Hermes Kanban board. This skill turns a commercial ask into the next usable artifact while preserving founder approval and team boundaries.

## When to Use

Use for:

- lead research, discovery preparation, follow-ups and proposals
- pipeline hygiene, deal unblockers and sales evidence
- internal growth experiments and campaign drafts
- work that crosses sessions, needs review, or depends on another card

Do not use the board as permission to send, publish, schedule, spend, alter a client account, or make unsupported customer claims.

## Board Contract

- **Board:** `tab-commercial`
- **Assignee:** `hermes-tab`
- **Tenant:** `tabtrickle`
- **Workspace:** the board default, a persistent commercial operations directory
- **Required grounding skill:** `tabtrickle-company`
- **Sign-off:** Justin or Spark before any client-facing or public use

Normal agent sessions use the `kanban_*` tools. A dispatcher-spawned worker begins with `kanban_show()` and stays pinned to its task and board. Do not shell out to `hermes kanban` from a worker.

## Card Standard

Every actionable card must state:

```markdown
## Commercial outcome
The deal movement, blockage removed, or copy-ready artifact required.

## Facts and sources
Known facts, links, records and the single marked gap, if any.

## Deliverable
The exact file, brief, message, proposal, CRM recommendation or decision required.

## Acceptance criteria
- [ ] The requested artifact exists and is usable.
- [ ] Claims are grounded in supplied context, the live site or cited research.
- [ ] External side effects remain unperformed unless the card contains exact approval.
- [ ] Verification evidence is recorded in the completion handoff.

## Approval gate
Who must approve, what exact action they are approving, and whether approval exists.
```

A vague card belongs in `triage`. A card with a usable brief belongs in `ready` unless a parent dependency keeps it in `todo`.

## Procedure

### 1. Read the card and its lineage

Call `kanban_show()` before doing work. Read parent results, comments, attachments and prior attempts. Completion criterion: the commercial outcome, required deliverable and approval state are explicit.

### 2. Resolve facts before asking

Inspect the CRM, live website, shared vault, supplied files or cited sources when relevant. Ask one question only when one unretrievable fact blocks the artifact; still draft with the gap marked where safe. Completion criterion: every factual claim is sourced or labelled as an assumption.

### 3. Work the commercial frontier

Produce the artifact, not a plan about producing it. During long work call `kanban_heartbeat()` with concrete progress. Create child cards with `kanban_create()` only when work is independently executable, crosses sessions, or belongs to another named profile. Link genuine blockers with `kanban_link()`.

Completion criterion: the card leaves behind something Justin can send, review, or walk into a meeting with.

### 4. Apply the approval gate

Use these routes:

- **Internal research, hygiene or draft:** complete when verified.
- **Client-facing/public artifact awaiting sign-off:** request review and name Justin or Spark in the summary.
- **Send, publish, schedule, spend or client-account mutation without exact approval:** call `kanban_block(kind="needs_input", ...)` and identify the exact action requiring approval.
- **Dependency unfinished:** call `kanban_block(kind="dependency", ...)` or create/link the parent card.
- **Missing capability or transient system fault:** block with the matching kind and include evidence.

Approval must match target, action and scope. General permission to prepare work is not permission to execute it externally.

### 5. Close with evidence

Call `kanban_complete()` only after verification. The summary must state the commercial result, not activity. Metadata should include, where applicable:

- `artifact_paths`
- `sources_checked`
- `verification`
- `approval_state`
- `external_actions` set to `none` unless read-back proves otherwise
- `next_move`

Scratch artifacts must be declared to `kanban_complete`; files in the persistent board workspace remain in place.

## Priority Rules

Rank ready work in this order:

1. live deal blocked on a reply, brief, scope or decision
2. time-bound discovery, proposal or follow-up
3. pipeline hygiene that exposes the next move
4. researched warm outreach in batches of five
5. draft growth experiments
6. speculative admin

Higher priority does not bypass approval.

## Team Boundaries

- Hermes Tab owns sales ops, daily commercial ops and non-social growth drafts.
- Spark owns CEO priority, routing and final quality control.
- Tab A0 owns marketing and social production.
- Justin is final human authority.
- Campaigns from Hermes Tab remain marked `DRAFT`.
- Do not create a parallel social team or assign live social work from this board.

## Pitfalls

- A card title is not a brief. Add the outcome, deliverable, criteria and gate.
- `done` means verified delivery, not "I wrote something".
- Do not duplicate CRM records as the board's source of truth. Link or cite the live record.
- Do not put credentials, private tokens or unnecessary personal data in cards, comments or shared files.
- Do not manufacture calls, replies, approvals, sends, results or customer claims.
- Avoid recurring automation until the manual card shape has produced clean, reviewed runs.

## Verification

Before closing a card, confirm:

- the artifact exists at the reported path or URL
- every acceptance criterion has evidence
- the approval state is explicit
- external state was read back after any authorised mutation
- the task summary gives the next operator a clear next move
